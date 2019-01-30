# polaris-logs
[![Build Status](https://travis-ci.com/Enigmatis/polaris-logs.svg?branch=develop)](https://travis-ci.com/Enigmatis/polaris-logs)
[![NPM version](https://img.shields.io/npm/v/@enigmatis/polaris-logs.svg?style=flat-square)](https://www.npmjs.com/package/@enigmatis/polaris-logs)

A node.js library that helps you create and use loggers according to a certain standard.

### LoggerConfiguration
Through this interface you should set the following configuration to the ``PolarisLogger``:

+ **loggerLevel** (*string*) - The level the logger is listening on, can be one of the following levels: ``fatal`` / 
``error`` / ``warn`` / ``info`` / ``trace`` / ``debug``
+ **logstashHost** (*string*)
+ **logstashPort** (*number*)
+ **writeToConsole** (*boolean - optional*) - Determines if the logger should write the logs to the console.
+ **writeFullMessageToConsole** (*boolean - optional*) - If you do decide to write logs to console, only the timestamp 
accompanied by the log level and message will be written, in order to see the whole log, which includes the properties 
that would be sent to the logstash, set this property to ``true``.
+ **logFilePath** (*string - optional*) - If provided, the logs will be written to the specified path.
+ **dailyRotateFileConfiguration** (*DailyRotateFileConfiguration - optional*) - If you are interested in daily log file
instead of just **one** log file, see the configuration section below. It creates a log file for each day. Those daily
log files deleted after ``X`` days after being created. **If provided, it ignores the logFilePath property.**

### DailyRotateFileConfiguration
+ **directoryPath** (*string*) - The directory path, where the daily files will be located.
+ **fileNamePrefix** (*string*) - The current date in the format of ``DD-MM-YYYY`` will be added to the name prefix.
+ **fileExtension** (*string*) - The extension of the log file (without the dot).
+ **numberOfDaysToDeleteFile** (*number - optional*) - Number of days till old log files will be deleted, default is 30
days.

### ApplicationLogProperties
This interface represent the application configurable log properties.

Those properties are:
 + systemId
 + systemName
 + repositoryVersion
 + environment
 + component

### PolarisLogProperties
This interface represent the log properties that will be logged through the ``PolarisLogger``.

### PolarisLogger
This class interacts with the actual winston logger and responsible for logging the properties that was provided to him.

### Example

```TypeScript

import { ApplicationLogProperties, LoggerConfiguration, PolarisLogger } from '@enigmatis/polaris-logs';

const appProps: ApplicationLogProperties = {
    id: 'p0laris-l0gs',
    name: 'polaris-logs',
    repositoryVersion: 'v1',
    environment: 'environment',
    component: 'component',
};

const logConf: LoggerConfiguration = {
    loggerLevel: 'trace',
    logstashHost: '127.0.0.1',
    logstashPort: 3000,
    writeToConsole: true,
    writeFullMessageToConsole: true,
    // logFilePath: 'D:\\example.log',
    dailyRotateFileConfiguration: {
        directoryPath: 'D:\\',
        fileNamePrefix: 'polaris',
        fileExtension: 'txt',
        numberOfDaysToDeleteFile: 60,
    },
};

const logger = new PolarisLogger(appProps, logConf);

logger.fatal('fatal message');
logger.error('error message');
logger.warn('warn message');
logger.info('info message');
logger.debug('debug message');
logger.trace('trace message');

```

For any additional help and requests, feel free to contact us :smile:
