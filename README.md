# Icinga Module for ASP.NET Core Health Checks

This module allows to query health checks from ASP.NET Core services, which use the libraries from [Xabaril/AspNetCore.Diagnostics.HealthChecks][0] and their Response writers:

## Requirements

Please install:

- [Curl][1]
- [jq][2]

## Examples

Given you configured your health checks like that in your ASP.NET Core Web Application:

```c#
config.MapHealthChecks("/health", new HealthCheckOptions
  {
     Predicate = _ => true,
     ResponseWriter = UIResponseWriter.WriteHealthCheckUIRespons
  });
```



Query the overall status:

```bash
check_aspnetcore_healthcheck --url http://servicename/health
```


Query the status of the health check named `PostgreSQL`

```bash
check_aspnetcore_healthcheck --url http://servicename/health --health-check-name "PostgreSQL"
```


## Minimal Example

Copy the `check_aspnetcore_healthcheck` to `/usr/lib/nagios/plugins`

Example in Icinga config:
```
object CheckCommand "aspnetcore_healthcheck" {
  command = [ PluginDir + "/check_aspnetcore_healthcheck" ]
  arguments = {
    "--url" = {
      value = "$url$",
      description = "The URL of the health check"
    },
    "--health-check-name" = {
      value = "$health-check-name$",
      description = "The name of the health check module, skip it to query overall status"
    }
  }
}

object Host "Host1" {
  address = "host1"
  check_command="hostalive"
}  

object Service "RssService" {
  host_name = "Host1"
  check_command = "aspnetcore_healthcheck"
  vars.url = "http://host1/health"
}
```
​	

[0]: https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks
[1]: https://curl.haxx.se/
[2]: https://stedolan.github.io/jq/