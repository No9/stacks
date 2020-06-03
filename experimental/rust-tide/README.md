# Rust Tide Stack

The Rust Tide stack provides a consistent way of developing [tide](https://github.com/http-rs/tide) http servers.

Designed to be used with [Appsody](https://appsody.dev/) an [open source](https://github.com/appsody/) development and operations accelerator for containers.

This stack is based on the `Rust 1.43.1` runtime.

## Templates

Templates are used to create your local project and start your development. When initializing your project you will be provided with the default template project. This template provides a http server that returns "Hello World" on http://localhost:8000/.

## Getting Started

1. Create a new folder in your local directory and initialize it using the Appsody CLI, e.g.:

    ```bash
    mkdir my-project
    cd my-project
    appsody init experimental/rust-tide
    ```
    This will initialize a Tide project using the default template.

1. After your project has been initialized you can then run your application using the following command:

    ```bash
    appsody run
    ```

    This launches a Docker container that continuously re-builds and re-runs your project. It also exposes port 8000 to allow you to call your service from your browser and test tooling.

1. You should see a message printed on the console:

```
    [Container] Server running on: http://localhost:8000/
```

1. Open a browser at http://localhost:8000/
     
     It should return `Hello, world`.

1. Now open lib.rs and change `world` to `Tide` and save the file.

    ```rust
        pub fn app() -> tide::Server<()> {    
            let mut api = tide::new();
            api.at("/").get(|_| async move { Ok("Hello, Tide!") });
            api
        }
    ```

1. Your application will be rebuild and republished so refresh http://localhost:8000/ it will now say `Hello, Tide`


## Debugging

To debug your application running in a container, start the container using:

```bash
    appsody debug --docker-options "--cap-add=SYS_PTRACE --security-opt seccomp=unconfined"
```

The command will start the [gdbgui](https://www.gdbgui.com/) platform and make a debugging environment available on port 5000. 

Once the environment is loaded open a browser at http://localhost:5000/

There is a known issue where the debugger [can't find the starting file](https://www.gdbgui.com/guides/#debugging-rust-programs)

So type the following in to the search bar.

```
/project/user-app/src/lib.rs
```

Set a breakpoint by clicking to the left of the line number.

Type run in the (gdb) prompt

Open a browser and call the service with http://localhost:8000 

The break point should be hit and the local variables will populate. 
Due to threading you may need to click the reload icon at the top right.

## License

This stack is licensed under the [Apache 2.0](./image/LICENSE) license