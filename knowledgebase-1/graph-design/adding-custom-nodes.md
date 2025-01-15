# Adding Custom Nodes

## 0. General information

Custom nodes can be separated into 3 groups:

* Function Library node - for straight-forward, purely async operations;
* Complex node - for operations that require a lot of flexibility;
* Simple ISPC node - for simple, high-performance operations.

## 1. Creating Function Library C++ nodes

To create Function Library C++ node, it is necessary to create `UVoxelFunctionLibrary` class, and create UFUNCTION functions. These will be visible within voxel graphs.

Function Library nodes are called in **Async** thread. As such, they cannot communicate with UObjects or UWorld.

Example:

```cpp
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "VoxelFunctionLibrary.h"

UCLASS()
class UTestFunctionLibrary : public UVoxelFunctionLibrary
{
	GENERATED_BODY()

public:
	UFUNCTION(Category = "Test")
	FVoxelFloatBuffer OneMinus(const FVoxelFloatBuffer& Value)
	{
		FVoxelFloatBufferStorage Storage;
		Storage.Allocate(Value.Num());
		for (int32 Index = 0; Index < Value.Num(); Index++)
		{
			Storage[Index] = 1.f - Value[Index];
		}
		return FVoxelFloatBuffer:Make(Storage);
	}
};
```

{% hint style="info" %}
Pure math operations such as this example are generally better off being done in ISPC, as this usually increases performance by a factor ten.
{% endhint %}

More examples can be found in:

`Source/VoxelGraphCore/(Public/Private)/FunctionLibrary/...`

## 2. Creating Complex C++ nodes

Complex C++ nodes can:

* Query information;
* Be calculated in several steps (i.e. wait for different pins or queries to collect information);
* Selectively be chosen to be processed in async/game threads;
* Have wildcard pins;
* Have custom logic on what to do when one pin is promoted.

#### 2.1. Header:

```cpp
USTRUCT(Category = "Custom")
struct FVoxelNode_MyCustomNode : public FVoxelNode
{
	GENERATED_BODY()
	GENERATED_VOXEL_NODE_BODY()

	// My tooltip as comment
	VOXEL_INPUT_PIN(float, CustomPin1, 5.f, AdvancedPin);
	VOXEL_INPUT_PIN(FVoxelFloatBuffer, CustomPin2, 1.f, DisplayName("My Custom Pin"), Category("Category A"));
	VOXEL_OUTPUT_PIN(FVoxelFloatBuffer, CustomOutputPin);

	// Optional
#if WITH_EDITOR
	virtual FVoxelPinTypeSet GetPromotionTypes(const FVoxelPin& Pin) const override;
	virtual void PromotePin(FVoxelPin& Pin, const FVoxelPinType& NewType) override;
#endif
};
```

Key elements in the header important are:

1. Have `GENERATED_VOXEL_BODY()` after the `GENERATED_BODY()` declaration;
2. Pins declarations.
3. Promotion - this is only necessary if you want pins to be promoted to different types;

#### 2.2. Pins:

Pins can be declared in four ways:

1. VOXEL\_**INPUT**\_PIN(**{TYPE}, {NAME}, {DEFAULT\_VALUE}, {META\_DATA}**);
2. VOXEL\_**OUTPUT**\_PIN(**{TYPE}**, **{NAME}, {META\_DATA}**);
3. VOXEL\*\*\_TEMPLATE\_INPUT\*\*\_PIN(**{TYPE}**, **{NAME}**, **{DEFAULT\_VALUE}, {META\_DATA}**);
4. VOXEL\_**TEMPLATE\_OUTPUT**\_PIN(**{TYPE}**, **{NAME}, {META\_DATA}**);

Explanation:

1. TEMPLATE - type pins means, that they will act together when converting from _Uniform_ to _Buffer_ and vice versa. If you promote one of Template pins container type, it will change all other template pins container type to match promoted pin container;
2. TYPE - will be pin type, some types have their matching buffer type, for example `float - FVoxelFloatBuffer`. If wildcard pin is necessary, then type can be created as `FVoxelWildcard` or `FVoxelWildcardBuffer` .
   1. **IMPORTANT!** Object type pins must have their own specific pin type created (more later);
3. DEFAULT\_VALUE - The value to use when unplugged. This can be left as nullptr for most types;
4. META\_DATA - Meta Data is optional. All meta data types are declared in `VoxelNode.h`, under the `FVoxelPinMetaDataBuilder` namespace. Some examples:
   1. DisplayName - Changes the pin’s user-facing name. _Accepts string_;
   2. ToolTip - Pin tooltip, visible when hovered;
   3. Category - Moves pin to a category. _Accepts string_;
   4. AdvancedPin - Moves pin under advanced group (collapsed-by-default dropdown);
   5. ArrayPin - Pin will be accept array type;
   6. ShowInDetail - Pin will be hidden by default and value will be shown in details panel (can be exposed as pin from details panel);

#### 2.3. CPP:

```cpp
#include "MyCustomNodeHeader.h"

// We define compute function by specifying our node struct name
// and output pin
DEFINE_VOXEL_NODE_COMPUTE(FVoxelNode_MyCustomNode, CustomOutputPin)
{
	// Each pin has to be registered for querying, by creating
	// variable TValue<Type> Value = GetNodeRuntime().Get(PIN, Query);
	// here we do not yet have the values from pins
	TValue<float> CustomPin1Value = GetNodeRuntime().Get(CustomPin1Pin, Query);
	TValue<FVoxelFloatBuffer> CustomPin2Value = GetNodeRuntime().Get(CustomPin2Pin, Query);

	// All pins we want to get values must be passed to VOXEL_ON_COMPLETE
	// To interact with UObjects or world, VOXEL_ON_COMPLETE_GAME_THREAD must be used instead.
	// ON_COMPLETE can be stacked inside.
	return VOXEL_ON_COMPLETE(CustomPin1Value, CustomPin2Value)
	{
		// Here value variables will have their values computed, so work can be done here

		// Since we want to return buffer, we have to create its storage
		FVoxelFloatBufferStorage ReturnValue;
		// And allocate necessary amount of values in it
		ReturnValue.Allocate(CustomPin2Value.Num());

		for (int32 Index = 0; Index < CustomPin2Value.Num(); Index++)
		{
			// We can write into BufferStorage by accessing necessary index
			// and we can read from normal Buffer by accessing index
			ReturnValue[Index] = CustomPin2Value[Index] + CustomPin1Value;
		}

		// We need to make Buffer from BufferStorage at the end
		return FVoxelFloatBuffer::Make(ReturnValue);
	};
}

// If promotion is required:
#if WITH_EDITOR
// This function allows to gather all allowed pin types for specific pin.
FVoxelPinTypeSet FVoxelNode_MyCustomNode::GetPromotionTypes(const FVoxelPin& Pin) const
{
	if (Pin.Name == ValuesPin)
	{
		return FVoxelPinTypeSet::AllBufferArrays();
	}
	else
	{
		return FVoxelPinTypeSet::AllBuffers();
	}
}

// This function is called when we want to promote specific pin
// if theres some additional work to be done with other pins, influenced
// by specific pin it must be done here.
void FVoxelNode_MyCustomNode::PromotePin(FVoxelPin& Pin, const FVoxelPinType& NewType)
{
	Pin.SetType(NewType);

	if (Pin.Name == ValuesPin)
	{
		GetPin(ResultPin).SetType(NewType.WithBufferArray(false));
	}
	else
	{
		GetPin(ValuesPin).SetType(NewType.WithBufferArray(true));
	}
}
#endif
```

The most important part here is to use **DEFINE\_VOXEL\_NODE\_COMPUTE** with your node struct name and output pin name passed to arguments.

To retrieve data from pins, querying must be done, using **return VOXEL\_ON\_COMPLETE(PinA, PinB….){}.** This part will put node execution into tasks pool and when all pin values are computed will execute inner lambda part. This can be stacked.

#### 2.4. Examples:

1. Source/VoxelGraphNodes/(Private/Public)/VoxelObjectNodes.(cpp/h)
2. Source/VoxelGraphNodes/(Private/Public)/VoxelArrayNodes.(cpp/h)
3. Source/VoxelGraphNodes/(Private/Public)/VoxelAppendNamesNode.(cpp/h)
4. Source/VoxelGraphNodes/(Private/Public)/VoxelAdvancedNoiseNode.(cpp/h)

#### 2.5. Querying parameter from context:

Nodes can convey information to downstream nodes as additional payloads on pins. This sort of additional information is called a QueryParameter. This is used within the plugin for information like Position and LOD.

{% hint style="info" %}
QueryParameters are only available to downstream nodes. Because graph execution is done right-to-left, a node is downstream if its output is plugged into another node’s input.
{% endhint %}

QueryParamaters can be retrieved using QueryParameter using **FindVoxelQueryParameter(ParameterType, VariableName);**

```cpp
DEFINE_VOXEL_NODE_COMPUTE(FVoxelNode_MyCustomNode, CustomOutputPin)
{
	FindVoxelQueryParameter(FVoxelPositionQueryParameter, PositionQueryParameter);
	FindVoxelQueryParameter(FVoxelLODQueryParameter, LODQueryParameter);

	FVoxelVectorBuffer Positions = PositionQueryParameter.GetPositions();
	int32 LOD = LODQueryParameter.LOD;
	....
};
```

#### 2.6. Creating query parameter and writing into it:

It is also possible to create custom QueryParameters, to allow downstream nodes to query custom data.

To do so, you need to create derived struct from **FVoxelQueryParameter** and create variables into which data will be stored:

```cpp
USTRUCT()
struct FVoxelCustomQueryParameter : public FVoxelQueryParameter
{
	GENERATED_BODY()
	GENERATED_VOXEL_QUERY_PARAMETER_BODY() // This is important

	FVector MyCustomData;
};
```

With the QueryParameter struct in place, we can configure the QueryParameters container to make use of it for the required pins. Keep in mind that QueryParameters are passed on a per-pin level, so it has to be declared for each pin individually.

```cpp
DEFINE_VOXEL_NODE_COMPUTE(FVoxelNode_MyCustomNode, CustomOutputPin)
{
	// First we clone existing query parameters
	const TSharedRef<FVoxelQueryParameters> NewQueryParameters = Query.CloneParameters();
	// Then add our custom query parameter with data
	NewQueryParameters->Add<FVoxelCustomQueryParameter>().MyCustomData = FVector(0.f, 1.f, 0.f);
	// Now it is necessary to compute pin with query data
	const TValue<float> CustomPin1Value = GetNodeRuntime().Get(CustomPin1Pin, Query.MakeNewQuery(NewQueryParameters));
	TValue<FVoxelFloatBuffer> CustomPin2Value = GetNodeRuntime().Get(CustomPin2Pin, Query);
	return VOXEL_ON_COMPLETE(CustomPin1Value, CustomPin2Value)
	{
		...
	};
}
```

In this example, CustomPin1 will have newly created CustomQueryParameter and CustomPin2 will not.

#### 2.7. Accessing UWorld:

It is important to access UWorld **only** from GameThread. To do that, first it is necessary to either run VOXEL\_ON\_COMPLETE\_GAME\_THREAD and access UWorld from there, or if you want to execute something (passing data, without returning results), you can use **FVoxelUtilities::RunOnGameThread(\[=]{ …. });**

To retrieve UWorld reference, **`FObjectKey WorldKey = Query.GetInfo(EVoxelQueryInfo::Query).GetWorld()`.** This will give FObjectKey, which **in GameThread** can be converted to UWorld pointer, by doing _UWorld World = Cast\<UWorld>(WorldKey.ResolveObjectPtr());_\*

{% hint style="info" %}
It is unsafe to access UWorld or any UObject from any threads other than the GameThread
{% endhint %}

Example:

```cpp
DEFINE_VOXEL_NODE_COMPUTE(FVoxelNode_MyCustomNode, CustomOutputPin)
{
	const TValue<float> CustomPin1Value = GetNodeRuntime().Get(CustomPin1Pin, Query);
	return VOXEL_ON_COMPLETE_GAME_THREAD(CustomPin1Value)
	{
		FObjectKey WorldKey = Query.GetInfo(EVoxelQueryInfo::Query).GetWorld();
		UWorld* World = Cast<UWorld>(WorldKey.ResolveObjectPtr());
		if (World)
		{
			....
		}
	};
}
```

## 3. Creating simple ISPC Node

ISPC nodes can be implemented without needing to write ISPC manually. To achieve this, create struct derived from `FVoxelISPCNode`:

```cpp
USTRUCT(Category = "Custom")
struct FVoxelNode_MyCustomNode : public FVoxelISPCNode
{
	GENERATED_BODY()
	GENERATED_VOXEL_NODE_BODY()

	VOXEL_TEMPLATE_INPUT_PIN(float, Value, nullptr);
	VOXEL_TEMPLATE_INPUT_PIN(float, Value2, nullptr);
	VOXEL_TEMPLATE_OUTPUT_PIN(float, ReturnValue);

	virtual FString GenerateCode(FCode& Code) const override
	{
		// This is useful when it is necessary to use some custom functions from already written functions in ISPC
		// Usually this is not necessary.
		Code.AddInclude("VoxelNoiseNodesImpl.isph");
		return "{ReturnValue} = min({Value} * {Value2}, {Value2} + {Value})";
	}
}
```

The plugin will look for these structs, and will generate `ISPC` code from the string returned by `GeneratedCode`. This type of node can only do relatively basic operations, but it does them very efficiently.

Examples:

1. Source/VoxelGraphNodes/Public/VoxelNoiseNodes.h
2. Source/VoxelGraphNodes/Public/Templates/…h

## 4. Calling ISPC in C++ Nodes

In itself, calling ISPC in C++ code is the same as calling global c++ functions. The main difference is to access them through **ispc** namespace and create ISPC code in \***.ispc** files.

First create ISPC function:

**MyCustomISPC.ispc**

```cpp
#include "VoxelMinimal.isph"

export void MyCustomNode_DoCalculation(const uniform float FloatBuffer[], const uniform int32 Num, uniform float ReturnBuffer[])
{
	FOREACH(Index, 0, Num)
	{
		// Do calculations
	}
}
```

And in the c++ node you want to call the ispc function, do:

```cpp
#include "**MyCustomISPC**.ispc.generated.h"

DEFINE_VOXEL_NODE_COMPUTE(FVoxelNode_MyCustomNode, CustomOutputPin)
{
	const TValue<FVoxelFloatBuffer> CustomPin2Value = GetNodeRuntime().Get(CustomPin2Pin, Query);
	return VOXEL_ON_COMPLETE_GAME_THREAD(CustomPin2Value )
	{
		const int32 Num = CustomPin2Value.Num();
		FVoxelFloatBufferStorage ReturnValue;
		ReturnValue.Allocate(Num);

		ForeachVoxelBufferChunk_Parallel(Num, [&](const FVoxelBufferIterator& Iterator)
		{
			ispc::MyCustomNode_DoCalculation(CustomPin2Value.GetData(Iterator), Num, ReturnValue.GetData(Iterator));
		};
	};
}
```

Including VoxelMinimal.isph in ispc file will allow access to our helper macros/functions like **FOREACH**, which helps to iterate buffers.

Examples:

1. Source/Private/VoxelGraphNodes/VoxelAdvancedNoiseNode.cpp (calls ISPC function)
2. Source/Private/VoxelGraphNodes/VoxelAdvancedNoiseNodesImpl.ispc (ispc functions declaration)
3. Source/Private/VoxelGraphNodes/VoxelHeightSplitterNode.cpp (calls ISPC function)
4. Source/Private/VoxelGraphNodes/VoxelHeightSplitterNodeImpl.ispc (ispc functions declaration)

## 5. Custom Pin Types:

Custom pin types can be divided into two:

1. Struct;
2. UObject pin type.

### 5.1. Struct Pin Type:

To use custom structs in graphs, it is necessary to define them as USTRUCT in C++ and create their buffer counterpart:

```cpp
USTRUCT()
struct FMyCustomStruct
{
	GENERATED_BODY()

	UPROPERTY()
	int32 A = 0;

	UPROPERTY()
	float B = 1;

	UPROPERTY()
	FVector C = FVector::ZeroVector;
};
```

Now for this struct to appear in voxel graphs, struct needs to be used in any of C++ nodes.

One way would be to define Make/Break nodes for this struct, other way would be to create any function and use this struct as parameter.

{% hint style="info" %}
Without Make/Break function nodes, struct can only be used in function arguments and you will not be able to split struct into separate pins.
{% endhint %}

{% hint style="danger" %}
Custom struct CANNOT contain UObjects.
{% endhint %}

{% hint style="info" %}
If custom struct contains other custom struct, that other struct also needs to have a buffer version implemented.
{% endhint %}

#### 5.1.1. Define custom struct buffer:

1. Declare it using **DECLARE\_BOXEL\_BUFFER(TypeBuffer, Type);**
2. Create buffer USTRUCT, derived from FVoxelBuffer;
3. Add buffer variation of every inner property;
4. Create \[] operator, to access uniform type at index position;
5. Create static Make function to create buffer from uniform value;
6. Create static Make function to create buffer from separated inner buffer values;
7. Override InitializeFromConstant function.

Example:

```cpp
DECLARE_VOXEL_BUFFER(FMyCustomStructBuffer, FMyCustomStruct);

USTRUCT()
struct FMyCustomStructBuffer final : public FVoxelBuffer
{
	GENERATED_BODY()
	GENERATED_VOXEL_BUFFER_BODY(FMyCustomStructBuffer, FMyCustomStruct);

	// Declare buffer variations of inner components
	UPROPERTY()
	FVoxelInt32Buffer A;

	UPROPERTY()
	FVoxelFloatBuffer B;

	UPROPERTY()
	FVoxelVectorBuffer C;

	// Define [] operator
	FORCEINLINE FMyCustomStruct operator[](const int32 Index) const
	{
		return FMyCustomStruct(A[Index], B[Index], C[Index]);
	}

	// Define Make function from uniform value
	static FMyCustomStructBuffer Make(const FMyCustomBuffer& Value)
	{
		FMyCustomStructBuffer Result;
		Result.A = FVoxelInt32Buffer::Make(Value.A);
		Result.B = FVoxelFloatBuffer::Make(Value.B);
		Result.C = FVoxelVectorBuffer::Make(Value.C);
		return Result;
	}

	// Define Make function from buffer components
	static FMyCustomStructBuffer Make(FVoxelInt32Buffer& InA, FVoxelFloatBuffer& InB, FVoxelVectorBuffer& InC)
	{
		FMyCustomStructBuffer Result;
		Result.A = InA;
		Result.B = InB;
		Result.C = InC;
		return Result;
	}

	// Overload InitializeFromConstant
	virtual void InitializeFromConstant(const FVoxelRuntimePinValue& Constant) override
	{
		A = FVoxelInt32Buffer::Make(Constant.Get<UniformType>().A);
		B = FVoxelFloatBuffer::Make(Constant.Get<UniformType>().B);
		C = FVoxelVectorBuffer::Make(Constant.Get<UniformType>().C);
	}
};
```

#### 5.1.2. Defining Make/Break functions:

Create UVoxelFunctionLibrary class and create two UFUNCTIONS, Make and Break.

It is important, that Make function would contain meta attribute **NativeMakeFunc** and break function would contain **NativeBreakFunc**

Example:

```cpp
UCLASS()
class UMyCustomStructFunctionLibrary : public UVoxelFunctionLibrary
{
	GENERATED_BODY()

public:
	UFUNCTION(Category = "Custom", meta = (NativeMakeFunc))
	FMyCustomStructBuffer MakeCustomStruct(const FVoxelInt32Buffer& A, const FVoxelFloatBuffer& B, const FVoxelVectorBuffer& C) const
	{
		// Need to perform check, are all buffers same size.
		CheckVoxelBuffersNum_Function(A, B, C);

		FMyCustomStructBuffer Result;
		Result.A = A;
		Result.B = B;
		Result.C = C;
		return Result;
	}

	UFUNCTION(Category = "Custom", meta = (NativeBreakFunc))
	void BreakCustomStruct(const FMyCustomStructBuffer& Value, FVoxelInt32Buffer& A, FVoxelFloatBuffer& B, FVoxelVectorBuffer& C)
	{
		A = Value.A;
		B = Value.B;
		C = Value.C;
	}
};
```

#### 5.1.. Custom Struct Pin Type ending notes:

After creating buffer and break/make types, custom struct type will be fully functional in voxel graphs.

### 5.2. Object pin types:

Voxel graphs does not support direct access to object pin types, since most of our execution is being done in async threads, which makes direct access unsafe and may crash.

To bypass direct access, we need to create custom struct as object pin type, which holds data, we want to access from object.

#### 5.2.1. Create object pin type:

1. Create custom struct;
2. Create object pin type struct, which reads data from UObject and stores it in custom struct;
3. Declare buffer type;
4. Make asset to data map.

Example:

```cpp
**HEADER**

// Our data asset
UCLASS()
class UTestDataAsset : public UPrimaryDataAsset
{
	GENERATED_BODY()

public:
	UPROPERTY(EditDefaultsOnly, Category = "Appearance")
	TObjectPtr<UStaticMesh> Mesh;

	UPROPERTY(EditDefaultsOnly, Category = "Appearance")
	FVector Scale;
};

// Struct which will hold data
USTRUCT()
struct FTestDataAssetData
{
	GENERATED_BODY()

	TWeakObjectPtr<UTestDataAsset> WeakObject;

	struct FData
	{
		FVoxelStaticMesh Mesh;
		FVecto Scale;
	};
	FData GetData() const;

	static FTestDataAssetData Make(UTestDataAsset* Asset);
};

// Declare object pin type
DECLARE_VOXEL_OBJECT_PIN_TYPE(FTestDataAssetData);

USTRUCT()
struct FTestDataAssetDataPinType : public FvoxelObjectPinType
{
	GENERATED_BODY()

	DEFINE_VOXEL_OBJECT_PIN_TYPE(FTestDataAssetData, UTestDataAsset)
	{
		if (bSetObject)
		{
			Object = Struct.WeakObject;
		}
		else
		{
			Struct = FTestDataAssetData::Make(Object.Get());
		}
	}
};

// Declare buffer type
DECLARE_VOXEL_TERMINAL_BUFFER(FTestDataAssetDataBuffer, FTestDataAssetData);

USTRUCT()
struct FTestDataAssetDataBuffer final : public FVoxelSimpleTerminalBuffer
{
	GENERATED_BODY()
	GENERATED_VOXEL_TERMINAL_BUFFER_BODY(FTestDataAssetDataBuffer, FTestDataAssetData);
};
```

```cpp
**CPP**

// Create critical section global variable
****FVoxelFastCriticalSection GTestAssetData_CriticalSection;
// Create global map to map asset with its struct data
TVoxelMap<FObjectKey, FTestDataAssetData::FData> GTestAssetData_AssetToData;

****FTestDataAssetData::FData FTestDataAssetData::GetData() const
{
		if (WeakObject.IsExplicitlyNull())
		{
			return {};
		}

		VOXEL_SCOPE_LOCK(GTestAssetData_CriticalSection);

		// Try to find data from global map
		const FData* Data = GTestAssetData_AssetToData.Find(MakeObjectKey(WeakObject));
		if (!ensure(Data))
		{
			return {};
		}

		return *Data;
}

FTestDataAssetData FTestDataAssetData::Make(UTestDataAsset* Asset)
{
	FTestDataAssetData Result;
	Result.WeakObject = Asset;

	if (Asset)
	{
		// Create data struct
		FData Data;
		Data.Mesh = FVoxelStaticMesh::Make(Asset->Mesh);
		Data.Scale = Asset->Scale;

		// Lock critical section
		VOXEL_SCOPE_LOCK(GTestAssetData_CriticalSection);
		// Store data to map
		GTestAssetData_AssetToData.FindOrAdd(Asset) = Data;
	}

	return Result;
}
```

#### 5.2.2. Use custom object pin type:

After creating custom object pin type, it is now possible to use the type in any of voxel nodes.

Example in Function Library:

```cpp
UCLASS()
class UTestDataAssetFunctionLibrary : public UVoxelFunctionLibrary
{
	GENERATED_BODY()

public:
	UFUNCTION(Category = "Custom")
	FVoxelStaticMeshBuffer GetMeshFromTestDataAsset(const FTestDataAssetDataBuffer& AssetBuffer)
	{
		FVoxelStaticMeshBufferStorage Storage;
		Storage.Allocate(AssetBuffer.Num());
		for (int32 Index = 0; Index < AssetBuffer.Num(); Index++)
		{
			Storage[Index] = AssetBuffer[Index].GetData().Mesh;
		}
		return FVoxelStaticMeshBuffer::Make(Storage);
	}

	UFUNCTION(Category = "Custom")
	FVoxelVectorBuffer GetScaleFromTestDataAsset(const FTestDataAssetDataBuffer& AssetBuffer)
	{
		FVoxelVectorBufferStorage Storage;
		Storage.Allocate(AssetBuffer.Num());
		for (int32 Index = 0; Index < AssetBuffer.Num(); Index++)
		{
			Storage[Index] = AssetBuffer[Index].GetData().Scale;
		}
		return FVoxelVectorBuffer::Make(Storage);
	}
};
```
