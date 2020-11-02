# Project Name
> ODAUtils-Rest

## Table of contents
* [General info](#general-info)
* [YAML Example](#code-example)
* [Contact](#contact)

## General info
A library that allows making REST calls.
Currently it supports:
* Protocol: 'HTTP' AND 'HTTPS'
* Method: 'GET' & 'POST'

## YAML Examples:
```
context:
  variables:
    greeting: "string"
    body: "string"
    response: "string"
    iResult: "nlpresult"

states:
  intent:
    component: "System.Intent"
    properties:
      variable: "iResult"
    transitions:
      actions:
        unresolvedIntent: "start"

  start:
    component: "System.List"
    properties:
      prompt: "Choose an sample"
      options:
      - label: "GET HTTP"
        value: "getHttp"
      - label: "GET HTTPS"
        value: "getHttps"
      - label: "POST HTTP"
        value: "postHttp"
      - label: "POST HTTPS"
        value: "postHttps"         
    transitions:
      actions:
        getHttp: callGetHttp
        getHttps: callGetHttps
        postHttp: setBodyPostHttp
        postHttps: setBodyPostHttps

---------------------------------------------------------------------------------------------------#
#                                          GET HTTP                                                 #
#---------------------------------------------------------------------------------------------------#
  callGetHttp:
    component: "com.oda.utilities.restconnector"
    properties:
      host: "http://jsonplaceholder.typicode.com"
      serviceURL: "/posts?userId=1"
      method: "GET"
      variable: "response"
    transitions:
      actions:
        success: "successState"
        fail: "failState"
#---------------------------------------------------------------------------------------------------#
#-                                          GET HTTPS                                              -#
#---------------------------------------------------------------------------------------------------#
  callGetHttps:
    component: "com.oda.utilities.restconnector"
    properties:
      host: "https://jsonplaceholder.typicode.com"
      serviceURL: "/posts?userId=1"
      method: "GET"
      variable: "response"
    transitions:
      actions:
        success: "successState"
        fail: "failState"
#---------------------------------------------------------------------------------------------------#
#-                                          POST HTTP                                              -#
#---------------------------------------------------------------------------------------------------#
  setBodyPostHttp:
    component: "System.SetVariable"
    properties:
      variable: body
      value:
        title: "foo"
        body: "bar"
        userId: 1
  callPostHttp:
    component: "com.oda.utilities.restconnector"
    properties:
      host: "http://jsonplaceholder.typicode.com"
      serviceURL: "/posts"
      body: ${body}
      method: "POST"
      variable: response
    transitions:
      actions:
        success: "successState"
        fail: "failState"
#---------------------------------------------------------------------------------------------------#
#-                                         POST HTTPS                                              -#
#---------------------------------------------------------------------------------------------------#
  setBodyPostHttps:
    component: "System.SetVariable"
    properties:
      variable: body
      value:
        title: "foo"
        body: "bar"
        userId: 1
  callPostHttps:
    component: "com.oda.utilities.restconnector"
    properties:
      host: "https://jsonplaceholder.typicode.com"
      serviceURL: "/posts"
      body: ${body}
      method: "POST"
      variable: response
    transitions:
      actions:
        success: "successState"
        fail: "failState"
#----------------------------------------------------------------------------------------------------

  successState:
    component: "System.Output"
    properties:
      text: "Call SUCCESS: ${response}"
    transitions:
      return: "done"
  failState:
    component: "System.Output"
    properties:
      text: "Call FAILED"
    transitions:
      return: "done"
 ```
## Contact
> Feel free to contact me for any support at ahmed.m.abdelghany@oracle.com
