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
`#metadata: information about the flow
#  platformVersion: the version of the bots platform that this flow was written to work with 
metadata:
  platformVersion: "1.1"
main: true
name: ODAUtilsPOST
#context: Define the variables which will used throughout the dialog flow here.
context:
  variables:
#The syntax for defining the variables is variablename: "variableType".
# The "variableType" can be defined as a primitive type ("string", "boolean", "int", "float", "double"), "list", "map", "resourcebundle", or an entity name. A variable can also hold the results returned by the Intent Engine. For these variables, the "variableType" must be "nlpresult" (for example, iResult: "nlpresult").
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
        # You can add an intent action here
        # e.g. Intent1 : "startIntent1"

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

#---------------------------------------------------------------------------------------------------#
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
#                                          GET HTTPS                                                #
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
#                                          POST HTTP                                                #
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
#                                          POST HTTPS                                               #
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
 `
## Contact
> Feel free to contact me for any support at ahmed.m.abdelghany@oracle.com 
