# UBlurpint

表示蓝图类

## 创建蓝图

核心函数 FKismetEditorUtilities::CreateBlueprint()

常规用法：
```c++
    UClass* BlueprintClass = UBlueprint::StaticClass();
    UPackage* BlueprintOuter = GetTransientPackage(); // 使用蓝图类所在的包作为Outer
    const FString DesiredName = FString::Printf(TEXT("EXPROUTCACHE_BP_%s"), *BlueprintClass->GetName());
    const FName BlueprintName = MakeUniqueObjectName(BlueprintOuter, BlueprintClass, FName(*DesiredName));
    BlueprintClass = FBlueprintEditorUtils::FindFirstNativeClass(BlueprintClass);

    UBlueprint *Blueprint = FKismetEditorUtilities::CreateBlueprint(UObject::StaticClass(), BlueprintOuter, BlueprintName, BPTYPE_Normal, BlueprintClass, UBlueprintGeneratedClass::StaticClass());
```

非常规：
```c++
    UClass* BlueprintClass = UBlueprint::StaticClass();
    BlueprintClass = FBlueprintEditorUtils::FindFirstNativeClass(BlueprintClass);

    UObject* Outer = GEditor; // 使用编辑器GEditor作为Outer，与GamePlay无关
    UBlueprint *Blueprint = FKismetEditorUtilities::CreateBlueprint(UObject::StaticClass(), Outer, "Test", BPTYPE_Normal, BlueprintClass, UBlueprintGeneratedClass::StaticClass());
```

## 编辑属性

核心函数 FBlueprintEditorUtils::AddMemberVariable()
类似的，移除属性、修改属性等操作，都由 FBlueprintEditorUtils 提供

```c++
    FEdGraphPinType PinType; // 属性类型相关见 UEdGraphPin.md
    PinType.PinCategory = UEdGraphSchema_K2::PC_Struct;
    PinType.PinSubCategory = NAME_None;
    PinType.PinSubCategoryObject = TBaseStructure<FVector>::Get();
    PinType.ContainerType = EPinContainerType::None;
    //PinType.PinValueType = ;
    FBlueprintEditorUtils::AddMemberVariable(Blueprint, "PropName", PinType);

    FKismetEditorUtilities::CompileBlueprint(Blueprint); // 修改完属性需编译蓝图
```

## 实例化蓝图类对象

```c++
    UObject* Object = NewObject<UObject>(Outer, Blueprint->GeneratedClass);
```

UObject 通过偏移量访问 UProperty 属性，内存大小和属性偏移量由 GeneratedClass 描述。UObject 实例一旦生成不能改变内存布局，不受 UBlueprint 变更的影响。 UObject 属性可以通过属性迭代器 ```TFieldIterator<FProperty> It(Blueprint->GeneratedClass)``` 遍历。 UBlueprint 变更后， GeneratedClass 与之前生成的对象失去关联，不能再通过新的 FProperty 信息计算变更前生成的 UObject 对象的属性偏移量。
