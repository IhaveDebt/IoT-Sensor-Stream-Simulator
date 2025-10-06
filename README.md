/**
 * iot_stream.ts
 *
 * Simulates IoT sensor data streams and publishes them to a pseudo-MQTT system.
 */

type SensorData = { id:string; temp:number; humidity:number; timestamp:number };

class IoTSimulator {
  subscribers:((data:SensorData)=>void)[]=[];
  subscribe(fn:(data:SensorData)=>void){ this.subscribers.push(fn); }
  publish(data:SensorData){ this.subscribers.forEach(fn=>fn(data)); }
  start(id:string,interval=1000){
    setInterval(()=>{
      const data:SensorData={id,temp:+(Math.random()*30).toFixed(2),humidity:+(Math.random()*100).toFixed(2),timestamp:Date.now()};
      this.publish(data);
    },interval);
  }
}

const sim=new IoTSimulator();
sim.subscribe(d=>console.log(`[${d.id}] T=${d.temp}°C H=${d.humidity}% @${new Date(d.timestamp).toISOString()}`));
sim.start('sensor-1',500);
