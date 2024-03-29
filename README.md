# checkbox

## Config:
| Name 				  | Type 					      | Default 			  | Description
| ---- 				  | :--: 					      | :-----: 			  | ---------------
| v-model 		  | `Boolean`				    | **false** 		  | Переключающееся значение
| label 				| `[String, Array]`   | **''** 			    | Заголовок для значения (если необходима череда ссылок и текста можно сформировать массив объектов text, link. Если link пуст вернется текст либо вернется ссылка)
| labelPosition | `String`				    | **'right'** 		| Позиция заголовка right|left
| checkPosition | `String`				    | **'center'** 		| Позиция заголовка top|start|center|bottom|end
| disabled 			| `Boolean`	          | **false** 			| Заблокировать переключатель
| viewType 			| `String`	          | **'circle'** 		| Выбрать тип переключателя из ['circle', 'square', 'rectangle']
| width 				| `[String, Number]`	| **24** 			    | Ширина переключателя
| height 			  | `[String, Number]`	| **24** 			    | Высота переключателя
| margin 			  | `[String, Number]`	| **'0 0 25 8'** 	| Внешнее смещение
| classes 			| `Array`				      | **[]** 			    | Массив классов
| elClass 			| `String`				    | **''** 			    | Пробрасывается класс на элемент чекбокса
| bColor 			  | `String`				    | **#1f1f33** 		| Цвет заливки чекбокса

## Example:
```html
<VCheckBox
  v-model="toggle"
  :label="[
    { text: 'Я согласен с' },
    { text: 'Публичной офертой', link: '/oferta' },
    { text: 'и' },
    { text: 'Политикой конфиденцальности', link: '/polit' },
  ]"
  labelPosition="left"
  width="20"
  height="20"
  margin="0 0 0 30"
  classes="c-checkbox"
/>

<!-- or -->

<VCheckBox
  label="Запомнить"
  v-model="toggle"
>
  <img :src="auth.rememberme
  	? `${require('./assets/img/yes.png')}`
	  : `${require('./assets/img/no.png')}`" alt="toggle"
	>
</VCheckBox>
```
