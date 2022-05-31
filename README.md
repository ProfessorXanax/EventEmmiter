Достаточно простой EventEmmiter для Rage и не только. Варианты использование ограничиваются только Вашей фантазией. Позволяет простейшим образом регистрировать прослушиватели событий и вызывать их. Очень полезен для использования в SPA. К примеру есть структура:
```
main.js
App.vue
–Hud.vue
—-Speedometer.vue
```
Достаточно сложно передавать данные используя execute внутрь компонента Speedometer.vue. Решается проблема достаточно просто:
main.js: 
```
import { createEventEmitter } from “./EventEmmiter.js”;
window.EventEmmiter = createEventEmitter(true);
```
Speedometer.vue: 
```
export default {
  data() {
    return {
      speed:0
    }
  },
  methods: {
    setSpeed(value) {
      this.speed=value;
    },
  },
  created() {
    window.EventEmmiter.on(“Speedometer:SetSpeed”,this.setSpeed);
  },
  beforeUnmount() {
    window.EventEmmiter.off(“Speedometer:SetSpeed”,this.setSpeed);
  },
}
```
И собственно с клиентской части вызов: 
```
browser.execute(`window.EventEmmiter.call(““Speedometer:SetSpeed”,2)`);
```
API:
add:
```
EventEmitter.add(string1,function1);
EventEmitter.add(string1,[function1,function2]);
EventEmitter.add({string1:function1,string2:function2});
EventEmitter.add({string1:[function1,function2],string2:[function1,function2]});
```
remove:
```
EventEmitter.remove(string1);
EventEmitter.remove(string1,function1);
EventEmitter.remove(string1,[function1,function2]);
EventEmitter.remove({string1:function1,string2:function2});
EventEmitter.remove({string1:[function1,function2],string2:[function1,function2]});
```
call:
```
EventEmitter.call(string1,arg1,arg2,...);
```
Есть возможность использовать как плагин для Vue3, что тоже очень упрощает задачу передачи данных в компоненты любой степени вложенности без использования Vuex/Pinia. Функция createEventEmitter используется для создания независимых копий. Возможно использование на Node.js клиентской и серверной части. Общая работа аналогична mp.events
