
# Notes

[According to this](https://docs.microsoft.com/en-us/azure/azure-functions/recover-python-functions?tabs=manual#enable-remote-build), the site needs `SCM_DO_BUILD_DURING_DEPLOYMENT` and `ENABLE_ORYX_BUILD` need to be enabled. I did that but the logs don't show any installation happening.

```log
2021-01-13T22:39:34.166 [Information] Initializing Warmup Extension.
2021-01-13T22:39:34.219 [Information] Initializing Host. OperationId: '2694506f-6a4e-49de-a8bb-8cf28eb314c8'.
2021-01-13T22:39:34.227 [Information] Host initialization: ConsecutiveErrors=0, StartupCount=1, OperationId=2694506f-6a4e-49de-a8bb-8cf28eb314c8
2021-01-13T22:39:34.272 [Information] ApplicationInsightsLoggerOptions{"SamplingSettings": {"EvaluationInterval": "00:00:15","InitialSamplingPercentage": 100.0,"MaxSamplingPercentage": 100.0,"MaxTelemetryItemsPerSecond": 20.0,"MinSamplingPercentage": 0.1,"MovingAverageRatio": 0.25,"SamplingPercentageDecreaseTimeout": "00:02:00","SamplingPercentageIncreaseTimeout": "00:15:00"},"SamplingExcludedTypes": "Request","SamplingIncludedTypes": null,"SnapshotConfiguration": null,"EnablePerformanceCountersCollection": true,"HttpAutoCollectionOptions": {"EnableHttpTriggerExtendedInfoCollection": true,"EnableW3CDistributedTracing": true,"EnableResponseHeaderInjection": true},"LiveMetricsInitializationDelay": "00:00:15","EnableLiveMetrics": true,"EnableDependencyTracking": true,"DependencyTrackingOptions": null}
2021-01-13T22:39:34.272 [Information] LoggerFilterOptions{"MinLevel": "None","Rules": [{"ProviderName": null,"CategoryName": null,"LogLevel": null,"Filter": "<AddFilter>b__0"},{"ProviderName": "Microsoft.Azure.WebJobs.Script.WebHost.Diagnostics.SystemLoggerProvider","CategoryName": null,"LogLevel": "None","Filter": null},{"ProviderName": "Microsoft.Azure.WebJobs.Script.WebHost.Diagnostics.SystemLoggerProvider","CategoryName": null,"LogLevel": null,"Filter": "<AddFilter>b__0"},{"ProviderName": "Microsoft.Azure.WebJobs.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider","CategoryName": null,"LogLevel": "Trace","Filter": null}]}
2021-01-13T22:39:34.273 [Information] LoggerFilterOptions{"MinLevel": "None","Rules": [{"ProviderName": null,"CategoryName": null,"LogLevel": null,"Filter": "<AddFilter>b__0"},{"ProviderName": "Microsoft.Azure.WebJobs.Script.WebHost.Diagnostics.SystemLoggerProvider","CategoryName": null,"LogLevel": "None","Filter": null},{"ProviderName": "Microsoft.Azure.WebJobs.Script.WebHost.Diagnostics.SystemLoggerProvider","CategoryName": null,"LogLevel": null,"Filter": "<AddFilter>b__0"},{"ProviderName": "Microsoft.Azure.WebJobs.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider","CategoryName": null,"LogLevel": "Trace","Filter": null}]}
2021-01-13T22:39:34.273 [Information] FunctionResultAggregatorOptions{"BatchSize": 1000,"FlushTimeout": "00:00:30","IsEnabled": true}
2021-01-13T22:39:34.276 [Information] SingletonOptions{"LockPeriod": "00:00:15","ListenerLockPeriod": "00:00:15","LockAcquisitionTimeout": "10675199.02:48:05.4775807","LockAcquisitionPollingInterval": "00:00:05","ListenerLockRecoveryPollingInterval": "00:01:00"}
2021-01-13T22:39:34.276 [Information] HttpOptions{"DynamicThrottlesEnabled": false,"MaxConcurrentRequests": -1,"MaxOutstandingRequests": -1,"RoutePrefix": "api"}
2021-01-13T22:39:34.281 [Information] Starting JobHost
2021-01-13T22:39:34.283 [Information] Starting Host (HostId=freeberg-python-func, InstanceId=fbb77879-1578-4a07-a327-df5dd9c85472, Version=3.0.15185.0, ProcessId=18, AppDomainId=1, InDebugMode=True, InDiagnosticMode=False, FunctionsExtensionVersion=~3)
2021-01-13T22:39:34.306 [Information] Loading functions metadata
2021-01-13T22:39:34.318 [Information] Loading proxies metadata
2021-01-13T22:39:34.320 [Information] Initializing Azure Function proxies
2021-01-13T22:39:34.491 [Information] 0 proxies loaded
2021-01-13T22:39:34.508 [Information] 1 functions loaded
2021-01-13T22:39:34.554 [Information] Generating 1 job function(s)
2021-01-13T22:39:34.621 [Information] Found the following functions:Host.Functions.PythonHttpTriggerFunction
2021-01-13T22:39:34.631 [Information] Initializing function HTTP routesMapped function route 'api/PythonHttpTriggerFunction' [get,post] to 'PythonHttpTriggerFunction'
2021-01-13T22:39:34.639 [Information] Host initialized (344ms)
2021-01-13T22:39:34.660 [Information] Host started (373ms)
2021-01-13T22:39:34.660 [Information] Job host started
2021-01-13T22:39:35.439 [Information] Worker process started and initialized.
2021-01-13T22:39:34.621 [Information] Found the following functions:Host.Functions.PythonHttpTriggerFunction
2021-01-13T22:39:34.631 [Information] Initializing function HTTP routesMapped function route 'api/PythonHttpTriggerFunction' [get,post] to 'PythonHttpTriggerFunction'
2021-01-13T22:39:34.639 [Information] Host initialized (344ms)
2021-01-13T22:39:34.660 [Information] Host started (373ms)
2021-01-13T22:39:34.660 [Information] Job host started
2021-01-13T22:39:35.439 [Information] Worker process started and initialized.
```

Finally, when the Function is executed here is the failure. Seems like `libGL.so` is not found.

```log
2021-01-13T22:07:11.973 [Error] Executed 'Functions.PythonHttpTriggerFunction' (Failed, Id=58c24ced-7b7d-4421-9b7e-e15815cd736b, Duration=4ms)Result: FailureException: ImportError: libGL.so.1: cannot open shared object file: No such file or directory. Troubleshooting Guide: https://aka.ms/functions-modulenotfoundStack:   
File "/azure-functions-host/workers/python/3.8/LINUX/X64/azure_functions_worker/dispatcher.py", line 271, in _handle__function_load_requestfunc = loader.load_function(
    File "/azure-functions-host/workers/python/3.8/LINUX/X64/azure_functions_worker/utils/wrappers.py", line 34, in callraise extend_exception_message(e, message)
    File "/azure-functions-host/workers/python/3.8/LINUX/X64/azure_functions_worker/utils/wrappers.py", line 32, in callreturn func(*args, **kwargs)
    File "/azure-functions-host/workers/python/3.8/LINUX/X64/azure_functions_worker/loader.py", line 76, in load_functionmod = importlib.import_module(fullmodname)
    File "/usr/local/lib/python3.8/importlib/__init__.py", line 127, in import_modulereturn _bootstrap._gcd_import(name[level:], package, level)
    File "<frozen importlib._bootstrap>", line 1014, in _gcd_import
    File "<frozen importlib._bootstrap>", line 991, in _find_and_load
    File "<frozen importlib._bootstrap>", line 961, in _find_and_load_unlocked
    File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
    File "<frozen importlib._bootstrap>", line 1014, in _gcd_import
    File "<frozen importlib._bootstrap>", line 991, in _find_and_load
    File "<frozen importlib._bootstrap>", line 975, in _find_and_load_unlocked
    File "<frozen importlib._bootstrap>", line 671, in _load_unlocked
    File "<frozen importlib._bootstrap_external>", line 783, in exec_module
    File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
    File "/home/site/wwwroot/PythonHttpTriggerFunction/__init__.py", line 6, in <module>import cv2 as cv
    File "/home/site/wwwroot/.python_packages/lib/site-packages/cv2/__init__.py", line 5, in <module>from .cv2 import *
```
