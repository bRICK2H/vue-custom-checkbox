# izi-checkbox-component

## Config:
| Name 				| Type 					| Default 			| Description 							|
| ---- 				| :--: 					| :-----: 			| --------------- 					|
| value 				| `Boolean`				| **false** 		| Переключающееся значение			|
| label 				| `String`				| **''** 			| Заголовок для значения			|
| labelPosition 	| `String`				| **'right'** 		| Позиция заголовка right|left	|
| width 				| `[String, Number]`	| **24** 			| Ширина переключателя				|
| height 			| `[String, Number]`	| **24** 			| Высота переключателя				|
| margin 			| `[String, Number]`	| **'0 0 25 8'** 	| Внешнее смещение					|
| classes 			| `Array`				| **[]** 			| Массив классов						|

## Example:
```html
<VCheckBox
  v-model="toggle"
  label="Запомнить"
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
	.>
</VCheckBox>
```
