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
aspnetcore_healthcheck http://servicename/health
```



Query the status of the health check named `PostgreSQL`

```bash
aspnetcore_healthcheck http://servicename/health "PostgreSQL"
```



​	

[0]: https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks
[1]: https://curl.haxx.se/
[2]: https://stedolan.github.io/jq/