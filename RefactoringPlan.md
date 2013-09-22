We are planning cleaning up and refactoring our code in 1.5. So we are listing tasks about that at this page.

- **DONE** Session keys should be managed at one place. 
- Lock keys for LockUtil should be managed at one place.
- **DONE** Should we introduce some control facilities such as ```using``` or ```defining```?
- Should we provide ```@error``` helper?
```
@error("mailAddress")
```
will generate
```
<span id="error-mailAddress" class="error"></span>
```
- **DONE** Use ```.string``` instead of ```<strong>``` in view.
- **DONE** ```repo/commit.scala.html``` and ```pulls/files.scala.html``` have same html fragment.
- **DONE** ```admin/users/group.scala.html``` and ```settings/collaborators.scala.html``` have same JavaScript fragment for user name completion field.
- **DONE** ```pulls/compare.scala.html``` and ```pulls/commits.scala.html``` have same html fragment.
- If the form have multiple actions, we want to use the formaction of HTML5.
- **DONE** The following code is used in many places. So it should be provided by implicit conversion for HttpServletRequest.
```
val paths = request.getRequestURI.substring(request.getContextPath.length).split("/")
```