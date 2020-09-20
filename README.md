# Variables
  body: "string"
  response: "string"

# POST
  setBody:
    component: "System.SetVariable"
    properties:
      variable: body
      value: 
        Parent1:
          Child1: "V1"
          Child2: "V1"
          ChildN: "V1"
          
  makePOSTCall:
    component: "com.oda.utilities.restconnector"
    properties:
      host: "http(s)://HOST-NAME:PORT"
      serviceURL: "RESOURCE PATH"
      body: ${body}
      variable: response
    transitions:
      actions:
        success: "successState"
        fail: "failState"
      
  successState:
    component: "System.Output"
    properties:
      text: "POST Call SUCCESS"
    transitions:
      return: "done"
  failState:
    component: "System.Output"
    properties:
      text: "POST Call FAILED"
    transitions:
      return: "done"      
