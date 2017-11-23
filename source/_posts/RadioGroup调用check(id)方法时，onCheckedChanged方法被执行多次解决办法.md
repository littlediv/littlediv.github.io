# RadioGroup调用check(id)方法时，onCheckedChanged方法被执行多次解决办法 #

1. 不在RadioGroup设置onCheckedChangedListener，而是给每个RadioButton设置OnClickListener
2. 给要开始显示的 radioButton 设置 setChecked(true)