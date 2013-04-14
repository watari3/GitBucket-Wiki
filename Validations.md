[Validations.scala](https://github.com/takezoe/gitbucket/blob/master/src/main/scala/util/Validations.scala) provides mapping and validation for request parameters. This is inspired by Play2 form mapping / validation.

At first, define the mapping as following:

```scala
import util.Validations._

val form = mapping(
  "name"        -> text(required, maxlength(40)), 
  "description" -> text()
)(RegisterForm.apply)
```

In the ScalatraServlet (or ScalatraFilter) implementation, you can validate request parameters using ```withValidation```. It validates request parameters before action and if any errors are detected, it throws an exception.

```scala
post("/register") {
  withValidation(form, params){ form: RegisterForm =>
    ...
  }
}

```

In the view template, you can add client-side validation by adding ```validate="true"``` to your form. Error messages are set to ```span#error-<fieldname>```.

```html
<form method="POST" action="/register" validation="true">
  Name: <input type="name" type="text">
  <span class="error" id="error-name"></span>
  <br/>
  Description: <input type="description" type="text">
  <span class="error" id="error-description"></span>
  <br/>
  <input type="submit" value="Register"/>
</form>
```

Also, you have to add a simple JSON API for validation to your ScalatraServlet (or ScalatraFilter) implementation. Client-side validation calls ```<form-action>/validate``` to validate form contents. In this case, form action is ```/register```, so you can ready ```/register/validate``` for client-side validation.

```scala
get("/register/validate") {
  contentType = "application/json"
  form.validateAsJSON(params)
}
```
