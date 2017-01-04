
https://12factor.net/

History of 12 factor
Why 12 factor

## I. Codebase
One codebase tracked in revision control, many deploys

Have seen multiple code bases built, and then bundled together in to a combined deployable.

Have seen the same code base being rebuilt for each environment to bake in different configuration.

## II. Dependencies
Explicitly declare and isolate dependencies

project.json makes it clear. new csproj files?

## III. Config
Store config in the environment

.env file?
environment variables

use Consul or others by injecting configuration at runtime, not querying it from the application

ensure configuration changes restart the service?

Use Consul client to reload configuration object and inject it via DI?
No long lived components should ensure that new configuration becomes active quickly.

## IV. Backing services
Treat backing services as attached resources

Don't do any fancy configuration work at this level, like service discovery. 

Inject via configuration, do any service discovery with consul or something in the orchestration layer.

## V. Build, release, run
Strictly separate build and run stages

dotnet test
dotnet publish

then use CI as a CD tool, or configuration management tool
in heroku example, `docker build -t myimage .` and push to docker registry or directly to heroku

## VI. Processes
Execute the app as one or more stateless processes

don't share resources with anything else, see IV.
no local caching
file system interface to reimplement against s3?

don't cache in memory. Don't do sticky sessions

    HttpContext.Session.SetString("Test", "Ben Rules!");

strings only
serialize to string yourself or: 

    var myComplexObject = new MyClass();
    HttpContext.Session.SetObjectAsJson("Test", myComplexObject);

    var myComplexObject = HttpContext.Session.GetObjectFromJson<MyClass>("Test");

## VII. Port binding
Export services via port binding

environment variables

aspnet?

{
  "server.urls": "http://localhost:60000;http://localhost:60001"
}

or PORT=8880
.UseUrls($"http://0.0.0.0:{PORT}")

## VIII. Concurrency
Scale out via the process model

not really applicable to the technology choices

just assume you are load balanced, even when you have no intention of doing so. At the very least, it's HA.

## IX. Disposability
Maximize robustness with fast startup and graceful shutdown

Don't pre-warm caches.
Keep services small.
Don't build on demand. - prebuild razor templates?
Compile for native.

## X. Dev/prod parity
Keep development, staging, and production as similar as possible

injection using configuration

## XI. Logs
Treat logs as event streams

Simple? Write To Console.

  // How to configure the console logger to use settings provided in code.
            //
            //
            //var settings = new ConsoleLoggerSettings()
            //{
            //    IncludeScopes = true,
            //    Switches =
            //    {
            //        ["Default"] = LogLevel.Debug,
            //        ["Microsoft"] = LogLevel.Information,
            //    }
            //};
            //factory.AddConsole(settings);


don't pull complex logging in to 

Complex:
- Serilog - provider for the Serilog library
- elmah.io - provider for the elmah.io service
- Loggr - provider for the Loggr service
- NLog - provider for the NLog library

## XII. Admin processes
Run admin/management tasks as one-off processes

Links to source code