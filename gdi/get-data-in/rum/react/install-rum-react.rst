.. _react-rum-install:

**************************************************************
Install the React Native RUM agent for Splunk RUM
**************************************************************

.. meta::
   :description: Instrument your React Native applications for Splunk Observability Cloud real user monitoring / RUM using the React Native RUM agent from the Splunk Distribution of OpenTelemetry for React Native.

You can instrument your React Native and Expo applications using the Splunk Distribution of OpenTelemetry for React Native. 

To instrument your React Native or Expo application and get data into Splunk RUM, follow the instructions on this page.

.. caution:: This distribution is currently in beta. Don't use it in production environments without extensive prior testing. Some features might not be supported or might have constrained capabilities.

.. _react-rum-requirements:

Check compatibility and requirements 
===============================================

Splunk RUM for Mobile supports React Native 0.67 and higher. 

The library is also compatible with the following frameworks and libraries:

- Expo framework
- React Navigation 5 and 6

.. note:: Splunk APM is not required to instrument Splunk RUM for React Native. 

.. _rum-react-install:

Instrument your React Native application for Splunk RUM
====================================================================

Before you instrument and configure Splunk RUM for your React Native application, understand which data RUM collects about your application and determine the scope of what you want to monitor. See :ref:`rum-data-collected`.

:strong:`Tip:` To generate all the basic installation commands for your environment and application, use the React Native Instrumentation guided setup. To access the React Native Instrumentation guided setup, follow these steps:

1. Log in to Observability Cloud.

2. In the left navigation menu, select :menuselection:`Data Management` to open the Integrate Your Data page.

3. In the integration filter menu, select :guilabel:`By Use Case`.

4. Select the :guilabel:`Monitor user experience` use case.

5. Click the :guilabel:`React Native Instrumentation` tile to open the React Native Instrumentation guided setup.

.. _rum-react-initialize:

Import and initialize the React Native RUM package
---------------------------------------------------------

Follow these steps to import and initialize the React Native RUM package.

1. Install the React Native RUM library using npm or yarn:

   .. code:: bash

      # npm
      npm install @splunk/otel-react-native

      # yarn
      yarn add @splunk/otel-react-native

   For iOS, also install the pod:

   .. code:: bash

      (cd ios && pod install)

2. Edit the initialization parameters to set the Observability Cloud realm, RUM access token, and basic attributes:

   .. code:: javascript

      const RumConfig: ReactNativeConfiguration = {
         realm: '<realm>',
         rumAccessToken: '<rum-access-token>',
         applicationName: '<your-app-name>',
         environment: '<your-environment>'
      }

3. Wrap your entire App component using the ``OtelWrapper`` component:

   .. code::

      import { OtelWrapper, startNavigationTracking } from '@splunk/otel-react-native';
      import type { ReactNativeConfiguration } from '@splunk/otel-react-native';

      const AppWithOtelWrapper = () => (
      <OtelWrapper configuration={RumConfig}>
         <App />
      </OtelWrapper>
      );

      export default AppWithOtelWrapper;

   Alternatively, you can initialize the library early in your code. See :ref:`react-alternative-init`.

4. (Optional) To instrument React Navigation, adapt your code as in the following example:

   .. code::

      import { startNavigationTracking } from '@splunk/otel-react-native';

      export default function App() {
         const navigationRef = useNavigationContainerRef();
         return (
            <NavigationContainer
               ref={navigationRef}
               onReady={() => {
                  startNavigationTracking(navigationRef);
               }}
            >
               <Stack.Navigator>
               // ...
               </Stack.Navigator>
            </NavigationContainer>
         );
      }

   For more information, see :new-page:`React Navigation <https://github.com/react-navigation/react-navigation>` on GitHub.

.. _react-alternative-init:

Alternative initialization method
----------------------------------------

As an alternative to wrapping the App component, you can initialize the React Native RUM library as early in your app lifecycle as possible. For example:

.. code:: javascript

   import { SplunkRum } from '@splunk/otel-react-native';

   const Rum = SplunkRum.init({
      realm: '<realm>',
      applicationName: '<name-of-app>',
      rumAccessToken: '<access-token>',
   });

.. _integrate-react-apm-traces:

Link RUM with Splunk APM
==================================

Splunk RUM uses server timing to calculate the response time between the front end and back end of your application, and to join the front-end and back-end traces for end-to-end visibility.

By default, the Splunk Distributions of OpenTelemetry already send the ``Server-Timing`` header. The header links spans from the browser with back-end spans and traces.

The APM environment variable for controlling the ``Server-Timing`` header  is ``SPLUNK_TRACE_RESPONSE_HEADER_ENABLED``. Set ``SPLUNK_TRACE_RESPONSE_HEADER_ENABLED=true`` to link to Splunk APM. 

How to contribute
=========================================================

The Splunk Distribution of OpenTelemetry for React Native is open-source software. You can contribute to its improvement by creating pull requests in GitHub. To learn more, see the :new-page:`contributing guidelines <https://github.com/signalfx/splunk-otel-react-native/blob/main/CONTRIBUTING.md>` in GitHub.