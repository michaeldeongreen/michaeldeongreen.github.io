---
layout: post
title:  "How to Send Azure Media Player Telemetry Information to Azure Application Insights in a Angular 7 Application"
date:  2019-02-28
categories: [technology]
images: michaeldeongreen-logo.png
tags: [Angular 7, Azure Media Player, Azure Application Insights]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

On one of my engagements here at Microsoft, we helped a Client Partner implement several Azure Backend components related to Azure Media Services. One of the requirements stated that the Client Partner's Frontend Angular application would need to send Azure Media Player telemetry information to [Azure Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview).  
  
In this blog I want to give a brief overview of the sample code I wrote that demonstrates how to use [ng-amp-diagnostics-logger](https://www.npmjs.com/package/ng-amp-diagnostics-logger) to send Azure Media Player telemetry information to [Azure Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview).  
  
ng-amp-diagnostics-logger is a package that sends Azure Media Player telemetry information to Azure Application Insights in a Angular application. ng-amp-diagnostics-logger is a TypeScript conversion of the [amp-diagnosticsLogger.js](https://github.com/Azure-Samples/media-services-javascript-azure-media-player-diagnostic-logger-plugin/blob/master/amp-diagnosticsLogger.js) JavaScript library.  
  
You can find the code used in this blog and how to run it on Github [here](https://github.com/michaeldeongreen/ng-amp-diagnostics-logger)  
  
**Overview!**  
  
The sample code is a basic Angular 7 Application that uses the Azure Media Player to stream sample media and send the telemetry information to Azure Application Insights.  
  
For brevity, please review the [README.md](https://github.com/michaeldeongreen/ng-amp-diagnostics-logger) file for the sample code, which provides a detailed breakdown of the [ng-amp-diagnostics-logger](https://www.npmjs.com/package/ng-amp-diagnostics-logger).  
  
**install**  
  
```bash
    
        npm install ng-amp-diagnostics-logger --save-dev
```

**app.component.ts**  
  
Import ng-amp-diagnostics-logger:  

```typescript
        import { NgAmpDiagnosticsLoggerService, NgAmpDiagnosticsLoggerConfiguration } from 'ng-amp-diagnostics-logger'
```

Inject NgAmpDiagnosticsLoggerService:  

```typescript
        constructor(private ngAmpDiagnosticsLoggerService: NgAmpDiagnosticsLoggerService) { }
```

Create dummy data to display on the screen. Url are official media sample from Azure Media Services:  

```typescript
        ngOnInit() {
            // Hardcoded list of mezzanine objects that are displayed to the user to play
            this.Mezzanines = [{ id: 'project-000-001', title: 'Movie 1', url: '//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest' },
            { id: 'project-000-002', title: 'Movie 2', url: '//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest' },
            { id: 'project-000-003', title: 'Movie 3', url:'//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest' }];
          }
    
```
  
One time configuration of Azure Media Player and one tme initialization of NgAmpDiagnosticsLoggerService by creating a new NgAmpDiagnosticsLoggerConfiguration object:  

```typescript
            ngAfterViewInit() {
                // Azure Media Player configuration
                var options = {
                  "nativeControlsForTouch": false,
                  autoplay: true,
                  controls: true,
                  width: "640",
                  height: "400"
                };
            
                // Get player object on app.component.html
                this.player = amp('vid1', options);
            
                // Configure NgAmpDiagnosticsLoggerConfiguration
                // Initialize NgAmpDiagnosticsLoggerService
                const ampDiagnosticsLoggerConfiguration: NgAmpDiagnosticsLoggerConfiguration =  {
                  appName: 'play-media-component',
                  player: this.player,
                  instrumentationKey: 'APPLICATION INSIGHTS INSTRUMENTATION KEY HERE'};
                  this.ngAmpDiagnosticsLoggerService.initialize(ampDiagnosticsLoggerConfiguration);
              }
    
```
  
setMovie is called from the UI when the user clicks a movie to play. Setting the Azure Media Player source and passing the _mezzanine.id_ to the _ngAmpDiagnosticsLoggerService_ service. This id will be set to the Application Insights _User Id_:  

```typescript
            public setMovie(mezzanine: Mezzanine) {
                this.player.src([{ src: mezzanine.url,
                type: 'application/vnd.ms-sstr+xml' }, ]);
                this.ngAmpDiagnosticsLoggerService.log(mezzanine.id);
              }
    
```
  
Screenshots:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-send-azure-media-player-telemetry-information-to-azure-application-insights-in-a-angular-7-application-001.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-send-azure-media-player-telemetry-information-to-azure-application-insights-in-a-angular-7-application-002.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-send-azure-media-player-telemetry-information-to-azure-application-insights-in-a-angular-7-application-003.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-send-azure-media-player-telemetry-information-to-azure-application-insights-in-a-angular-7-application-004.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-send-azure-media-player-telemetry-information-to-azure-application-insights-in-a-angular-7-application-005.png)
