var status = {};
let statusBPortail = Virtual.getHandle("boolean:200");
let statusMesureBPortail = Virtual.getHandle("boolean:201");
let statusNPortail = Virtual.getHandle("number:200");
let timer = 0;
let trigger = true;
let lastInputChange = 0;
let triggerInput0 = true;
let triggerInput1 = true;
let triggerInput0Used = true;
let triggerInput1Used = true;
      
// Subscribe event to retrieve temperature and input
Shelly.addStatusHandler(function(e,u) {
  if (e.component === "switch:0") {
    if(e.delta.output == true) {
      timer = Shelly.getComponentStatus("sys").unixtime;
      trigger = false;
      triggerInput0 = false;
      triggerInput1 = false;
    }
  }
  
  if (e.component === "input:0") {
    //print("Evt input 0");
    if(e.delta.state == true && triggerInput0 == false) {
      result = Shelly.getComponentStatus("sys")
      status.input0Time = result.unixtime;
      triggerInput0 = true
      triggerInput0Used = false;
      //print(status.input0Time);
    }
  }
  if (e.component === "input:1") {
    //print("Evt input 1");
    if(e.delta.state == true && triggerInput1 == false) {
      result = Shelly.getComponentStatus("sys")
      status.input1Time= result.unixtime;
      triggerInput1 == true;
      triggerInput1Used = false;
      //print(status.input1Time);
    }
  }
  
  if(e.component === "boolean:200" && trigger == true && timer + 60 < Shelly.getComponentStatus("sys").unixtime ) {
    print("boolean:200 Update");
    Shelly.call("switch.set", { id: 0, on: true}, function (result, code, msg, ud) { print("Switch:0 result ", result); }, null);
  }
  
  print("Delta status.input0Time - status.input1Time", Math.abs(status.input0Time - status.input1Time), " trigger ", trigger);
  if(Math.abs(status.input0Time - status.input1Time) < 3 && trigger == false && triggerInput0Used == false && triggerInput1Used == false ) {
    statusBPortail.setValue(false);
    statusMesureBPortail.setValue(false);
    trigger = true;
    triggerInput0Used = true;
    triggerInput1Used = true;
    print("status.input0Time < status.input1Time set True - Ouvert");
  }
  if(Math.abs(status.input0Time - status.input1Time) > 3 && trigger == false && triggerInput0Used == false && triggerInput1Used == false ) {
    statusBPortail.setValue(true);
    statusMesureBPortail.setValue(true);
    trigger = true;
    triggerInput0Used = true;
    triggerInput1Used = true;
    print("status.input0Time > status.input1Time set False - Fermé");
  }
  
  //print("status: ", e.component, " => ", JSON.stringify(e), " at ", Shelly.getComponentStatus("sys").unixtime);
  //print(e);
});
