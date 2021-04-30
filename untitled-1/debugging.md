# Debugging

## Debugging a Server Error

**Question:** What should I do when I come across an internal error \(like the one below\) while running Open Library locally on Docker?

```text
"Sorry. There seems to be a problem with what you were just looking at. We've noted the error 2019-02-05/193353339339 and will look into it as soon as possible. Head for home?"
```

**Answer:** When you hit an error, if you add `?debug=true` to the url \(if it's a GET\), or \(if it's a POST\) inspect the form element and add `?debug=true` to the action url, you should see a useful stack trace.

## Using a Debugger

BETA: See [https://github.com/internetarchive/openlibrary/pull/2097](https://github.com/internetarchive/openlibrary/pull/2097)

Using a debugger is one of the best ways to understand how code works and to find bug fixes. We use [VS Code](https://code.visualstudio.com/) \(along with its [Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python)\) to remote debug into the docker container; we can't use regular debugging because the docker container behaves like a virtual machine.

![Debugger in action](https://user-images.githubusercontent.com/6251786/56706388-bd889e00-66e2-11e9-9d9b-449f0458305a.gif)

To use: 1. `docker-compose down` 2. Open the repo in VS Code -- `code .` 3. Edit around line 8 of `docker-compose.yml` to say `workers 1` instead of `4` 4. `docker-compose up -d` as usual and [**login as admin**](https://github.com/internetarchive/openlibrary/wiki/Getting-Started#logging-in). \(see [https://github.com/internetarchive/openlibrary/issues/2122](https://github.com/internetarchive/openlibrary/issues/2122)\) 5. Go to [http://localhost:8080/admin/attach\_debugger](http://localhost:8080/admin/attach_debugger) \(or whatever your docker ip is\) and click "Start" 6. Go to the debug panel \(Ctrl+Shift+D\) in VS Code, and click "Python: Attach" 7. Debug!

Admin login troubles:

* Should login as openlibrary@example.com / admin123
  * [https://github.com/internetarchive/openlibrary/blob/master/CONTRIBUTING.md\#logging-in-as-admin](https://github.com/internetarchive/openlibrary/blob/master/CONTRIBUTING.md#logging-in-as-admin)
* If you ever logged in with something else, it goes a little crazy; reset your docker volumes: `docker-compose build web` and then log in again
* If you're running Linux ensure that a docker group is created, and you are a member of the group. If you are not a member of the group you may not be able to access admin while logging in.

  \(TODO: Move this to a better documentation page\)

