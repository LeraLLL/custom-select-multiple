# custom-select

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

### Немного о проекте 

* Реализован выпадающий список`select` с функцией `multiple` без настивного тега `select`
* На вход компонет получает `options`, на выходе мы получаем массив из `values`
* Выбранные позиции в селекте отображаются в виде `chips`, a в `options` они указаны активными чекбоксами
* Удалить позицию из выбранных можно или нажав на инпут потом на `backspace` , отжав нужный чекбокс в `options` или нажав на крестик в `chip`
* Реализован поиск по названию в списке элементов
