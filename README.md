# vue3-dialogs
Web app dialog components: modal + toast developed for Vue 3

Finally I get some time to play with the Vue 3 CLI. Vue 3 looks great and I just scratch the surface, this time I decided to develop a pair of 'always used' components: a modal dialog and a toast. These elements need interact from parent (to send data) and the component itself (to run inner methods/events).

If you use the Vue CLI basically you can't feel the difference between v2 and v3, because the CLI provides you the blueprint but if you use develop SSR (server side rendering apps) you need to face a little new way to instantiate your app, the old straight way to instantiate your app:

```bash
const app = new Vue({ definition here... });
console.log(app.someAttribute);
app.someMethod();
```

changed to:

```bash
const app = Vue.createApp({ definition here... });
```

then mount it:

```bash
const mountedApp = app.mount('#vue-app');
```

so, if externally you need access to some Vue instance property or method you must use 'mountedApp' not 'app':

```bash
console.log(mountedApp.someAttribute);
mountedApp.someMethod();
```

Back to these components, check the samples please, but in short you need to call dialog and modal using "$refs" then using component method, in the case of toast component:

```bash
this.$refs.toastRefName.text({
  message: "This is the toast message!",
  type: "success", //optional, if not accepts: 'success', 'error' and 'warning'
  callback: this.myToastCallback //this is optional too
})
```
in the other case, you need to invoque the .show() method:

```bash
this.$refs.modalRefName.show({
  title: "ALERT", //optional, the dialog title
  message: "Your awesome modal message here!", //the only required property
  buttons: {
    ok: {
      callback: () => {
        console.log("Default modal callback..");
      },
    },
  },
})
```

There is a live version: https://experiments.oscaralderete.com/vue3-dialogs/
