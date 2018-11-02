## Акторы
Акторы служат мостом между фреймворком и физическим объектом в Unity. Задача актора визуально настроить базовые компоненты сущности, инициализировать сущность. При уничтожении игрового объекта с актором уничтожается и сущность.

## Как создавать.
Акторы наследуются от класса Actor или MonoActor. MonoActor отличается возможностью добавления апдейтов и сигналов.
```csharp
 // basic melee unit
    public class ActorEnemy : Actor
    {

        // Атрибут FoldoutGroup("ИМЯ") - нарисует компоненты внутри сворачиваемой группы с заданным именем.
        [FoldoutGroup("Setup")] public ComponentDamageble componentDamageble;
        [FoldoutGroup("Setup")] public ComponentAiBasic componentAIBasic;  
        [FoldoutGroup("Setup")] public ComponentRigidBody componentRigidBody;
        [FoldoutGroup("Setup")] public ComponentBounds componentBounds;
        [FoldoutGroup("Setup")] public ComponentDepth componentDepth;
        [FoldoutGroup("Setup")] public ComponentShadow componentShadow;
        [FoldoutGroup("Setup")] public ComponentView componentView;
        [FoldoutGroup("Setup")] public ComponentAnimationMap componentAnimationMap;
        [FoldoutGroup("Setup")] public ComponentHealth componentHealth;


        // метод setup служит инициализацией, он отыгрывается после Awake. Именно через сетап мы передаем компоненты сущности.
        protected override void Setup()
        {
     
            // Используем Add чтобы передать компонент сущности, не поощряется, но компонент может быть унаследован от другого компонента и в качестве типа может быть указан базовый тип ( родитель ). 
            Add(componentAIBasic as ComponentAI);
            Add(componentShadow);
            Add(componentView);
            Add(componentBounds);
            Add(componentDepth);
            Add(componentRigidBody);
            Add(componentDamageble);
            Add(componentHealth);

            Add<ComponentTime>();
            Add<ComponentObjectBounds>();
 
            Add(Tag.GroupEnemy, Tag.GroupDestructable);
        }
}
```


```csharp
        Add(Path.ToHitSpot, "View/Point Hit");
            Add(Path.ToHitNormal, "View/Collider Hit");
            Add(Path.ToHitCritical, "View/Collider Hit Critical");
            Add(Path.ToSolid, "View/Collider Solid");
            Add(Path.ToView, "View");
```