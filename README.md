# Dynatrace Ionic 2 EasyTravel Demo Application
> Ionic 2 + Electron Sample Apps

![Screenshot](docs/screenshot.PNG?raw=true "Screenshot of the app")

The EasyTravel demo application is used to explain how Dynatrace real user monitoring within a typical Cordova Ionic 2 hybrid app works. EasyTravel implements a simple travel booking app that triggers real backend service calls. The user is able to book journeys within the application. This is only a demo use case (no worry, we do not charge for the journey you are booking). The goal of this demo application is to show you how easy it is, to instrument a hybrid ionic application with Dynatrace and how easy it is to track the actions of the user within the application.

## Getting Started

This section describes how to install and run this demo application on your own computer or mobile phone.

### Prerequisites

The following software packages are necessary to build and run the EasyTravel app.

1. Installation of NodeJS: https://nodejs.org/en/
2. Installation of required global node modules with the following command: `npm install -g ionic corsproxy grunt-cli`

### Installation

Now you are ready for the installation of the application.

1. Checkout or download the project content
2. Installation of all necessary project modules: In the project directory execute the command `npm install`
3. Add a desktop platform: Enter the command `ionic platform add browser`
4. **Not necessary**: Add a mobile platform (e.g. Android) - Enter the command `ionic platform add Android`

**Important:** Especially when adding a platform be sure that you install all necessary platform tools. See https://ionicframework.com/docs/v2/getting-started/installation/ for more information. The Android platform requirements can be found here: http://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html. The iOS platform requirements can be found here: http://cordova.apache.org/docs/en/latest/guide/platforms/ios/index.html

### Start

Depending on the platforms added, the startup looks different. Basically `grunt serve-lab` & `grunt serve-web` will work out of the box, because they do not need a platform to be executed. (If you want to run `ionic serve --lab` instead, please read the section *CORS Problems*.)

If you want to run the application on iOS or Android simply enter `ionic run android` or `ionic run ios`. This, of course, only works when the platform was added in the installation step.

Run `grunt serve-app` to start the Electron demo app.

### Build

To build the iOS & Android apps, execute `ionic build ios` or `ionic build android`, respectively. Again, this only works when the platform was added in the installation step.

To build a simple web app, just run `grunt build-web`.

To create an Electron app package, run `grunt build-app`.

### CORS Problems

If you want to start the application in your browser locally hardly any browser will allow CORS requests. To bypass this symptom you can install an extension for your browser. This [Chrome extension](https://chrome.google.com/webstore/detail/cors/dboaklophljenpcjkbbibpkbpbobnbld?utm_source=chrome-app-launcher-info-dialog) is known to work. You have to turn on the extension after installing it. This workaround is not needed for testing on the phone.

It's cleaner, however, to use a CORS proxy. To start the proxy manually, execute `corsproxy` in a command line. **The Grunt "serve" tasks automatically start it in the background.**

Now you can navigate to the *Settings* page and enter "localhost" in the *Proxy Host* field, as well as the port number used by the CORS proxy (displayed in the command line window) in the *Proxy Port* field.

## Instrumentation

The instrumention is fast and easy and supports both Ionic 1 and Ionic 2. Make sure that you started the application at least one time (as described above), to be sure that the application is working on your computer. Following steps will automatically instrument the Ionic application:

* Execute the command *npm install dynatrace-ionic-instrumentation* in your project folder.
* The *instrument.properties* file will now appear in your root project folder after the installation of the instrumentation tool has finished.
* Fill in the configuration in the *instrument.properties* with the information of your own dynatrace environment.

The property file (instrument.properties) looks like this:

```
## Instrumentation Properties - Please define in order to download the JS Agent ##
SOURCE_DIRECTORY = "src"

# General Settings - 'DYNATRACE SAAS' 'DYNATRACE MANAGED' OR 'DYNATRACE APPMON'
TYPE = "DYNATRACE SAAS"

# (ONLINE / Include agent or OFFLINE / Download agent on the fly)
AGENT_LOCATION = "OFFLINE"

# DYNATRACE SaaS/Managed API Access 
ENVIRONMENT_ID = "fdq95078" 
CLUSTER_URL = "fdq95078.dev.dynatracelabs.com"
APPLICATION_ID = "203B8CC2009EE62F"
API_TOKEN = "H0yp6L_QSFyi3iaphSBJX"

# APPMON API Access
APPMON_URL = ""
APPLICATION_NAME = "easyTravel portal"
PROFILE = "easyTravel"
API_USER = "admin"
API_PASSWORD = "admin"
```

This is the download and default version of the properties. Following points have to be configured:

* SOURCE_DIRECTORY: The source directory at the beginning is the source directoy of your Ionic project. For most Ionic 2 projects this directory is called *"src"*. In Ionic 1 this directory can also be called *"www"*. 
* TYPE: Dynatrace Saas is configured as default type. Other type options might be Dynatrace Managed or Dynatrace AppMon. 
* AGENT_LOCATION: You can choose wether you want to download the whole agent (OFFLINE) and include the agent in the build or if you want to add only a tag (ONLINE) in the HTML which is downloading the agent on the fly (when the application is starting). 
* Beneath the other options you can find the API configuration for Dynatrace Saas/Managed and Dynatrace AppMon. Depending on which TYPE you use, make sure that you fill in the configuration. 

You are done! After those two simple and short steps your project will be instrumented automatically by our scripts when you build the application. Be aware that the used agent is only working when you make a native build (Android, iOS) [e.g. *ionic build android*]. The command *ionic serve --lab* will therefore sadly not work. If you use *ionic serve --lab* the application would still be instrumented and will start as usual, but the agent can not communicate with the server (Because of CORS, as described above). 

## Credits

* Icons: [Flat Icons](http://www.flaticon.com/authors/flat-icons), [Freepik](http://www.flaticon.com/authors/freepik), [Eleonor Wang](http://www.flaticon.com/authors/eleonor-wang), [Dave Gandy](http://www.flaticon.com/authors/dave-gandy) from [Flaticon.com](http://www.flaticon.com) is licensed by [CC 3.0 BY](http://creativecommons.org/licenses/by/3.0/)
