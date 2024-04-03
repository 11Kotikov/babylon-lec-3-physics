# Лекция 3
## Что изменилось/добавилось:
### Изменения в HTML:

```html
ничего
```

### Изменения в CSS:


```css
ничего
``` 

### Изменения в JavaScript:

```javascript
const createPlatform = (zPos) => {
    ///....
      // Добавляем физический импостер платформе
    platform.physicsImpostor = new BABYLON.PhysicsImpostor(
        platform,
        BABYLON.PhysicsImpostor.BoxImpostor,
        {
            mass: 0 // Устанавливаем массу платформы равной 0 (таким образом, она не будет двигаться под воздействием физики)
        },
        scene
    );
    /////
}
// Работа с физикой мяча
    ball.physicsImpostor = new BABYLON.PhysicsImpostor(
        ball,
        BABYLON.PhysicsImpostor.SphereImpostor,
        {
            //набор физических свойств
            mass: 1,
            //restitution: 2, //отскок
            friction: 5, //трение
        },
        scene
);

window.addEventListener('touchstart', (event) => {
    let x = event.touches[0].clientX; // Получаем горизонтальную координату первого касания

    if (x > (window.innerWidth / 2)) { // Если касание произошло правее центра экрана
        ball.physicsImpostor.applyImpulse(
            new BABYLON.Vector3(2, 0, 0), // Применяем импульс вправо
            ball.getAbsolutePosition() // Точка импульса = текущая позиция мяча
        );
    } else { // Если касание произошло левее или в центре экрана
        ball.physicsImpostor.applyImpulse(
            new BABYLON.Vector3(-2, 0, 0), // Применяем импульс влево
            ball.getAbsolutePosition() // Точка импульса = текущая позиция мяча
        );
    }
});

window.addEventListener('touchend', () => {
    ball.physicsImpostor.setLinearVelocity(new BABYLON.Vector3(0, 0, 5));
    ball.physicsImpostor.setAngularVelocity(new BABYLON.Vector3(0, 0, 0));
});

window.addEventListener('beforeinstallprompt', (event) => {
    event.preventDefault();
});

//Алгоритм
//рендер сцены
engine.runRenderLoop(()=> {
    camera.position.z = ball.getAbsolutePosition().z - 12;
    light.position.z = ball.getAbsolutePosition().z;
    scene.render();
});

```
1. Создание платформы:

    В новой версии была добавлена функция createPlatform, которая создает платформу с текстурой и физическим импостером.
    Эта функция вызывается в цикле для создания 10 платформ на разных позициях по оси Z.

2. Физика мяча:

    Для мяча была добавлена физическая модель с использованием PhysicsImpostor, определяющая его массу и трение.
    Добавлены обработчики событий touchstart и touchend, которые обрабатывают события касания для изменения движения мяча.

3. Алгоритм обновления позиции камеры и света:

    Добавлен алгоритм, который автоматически обновляет позицию камеры и света в зависимости от позиции мяча.
    Это позволяет камере следовать за мячом и освещать его правильно.