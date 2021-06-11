# vue3-dialogs
Web app dialog components: modal + toast developed for Vue 3

Finally I get some time to play with the Vue 3 CLI. Vue 3 looks great and I just scratch the surface, this time I decided to develop a pair of 'always used' components: a modal dialog and a toast. These elements need interact from parent (to send data) and the component itself (to run methods/events).

If you use the Vue CLI basically you can't feel the difference between v2 and v3, because the CLI provides you the blueprint but if you use develop SSR (server side rendering apps) you need to face a little new way to instantiate your app, the old straight way to instantiate your app:
```bash
const app = new Vue({ definition here... });
```
changed to:
```bash
const app = Vue.createApp({ definition here... });
```
then mount it:
const realApp = app.mount('#vue-app');
so, if externally tou need to access to some Vue instance property or method you must use 'realApp' not 'app':
realApp.someAttribute;
realApp.somemethod();



There is a live version: https://experiments.oscaralderete.com/vue3-dialogs/
