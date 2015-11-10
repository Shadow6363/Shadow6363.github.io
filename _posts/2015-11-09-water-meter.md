---
layout: post
title: Live Wi-Fi Water Meter Reader
excerpt: ""
modified: 2015-11-09
tags: [photon, water meter, iot]
comments: true
pinned: true
---

<p>
So this is a project that I started way back in 2012. For a while I had thought I'd be able to make a Kickstarter out of it; however, I've finally realized I just don't have the time or know-how.
</p>

### The Problem
<p>
It started out when the water bill seemed higher than it should be, and I wanted to know <em>why</em>.
Unfortunately, even with their "smart" meter, the water company's reporting was at best, daily. This didn't really help me figure out what was using all of the water.
</p>

<div id="carousel-meter" class="carousel slide" data-ride="carousel">
  <!-- Indicators -->
  <ol class="carousel-indicators">
    <li data-target="#carousel-meter" data-slide-to="0" class="active"></li>
    <li data-target="#carousel-meter" data-slide-to="1"></li>
    <li data-target="#carousel-meter" data-slide-to="2"></li>
  </ol>

  <!-- Wrapper for slides -->
  <div class="carousel-inner" role="listbox">
    <div class="item active">
      <img src="{{ site.url }}/img/{{ page.id | remove_first:'/' | replace:'/','-' }}/a182251990798ad9238ad76062e94ef8.jpg" alt="Water Meter Cabinet">
      <div class="carousel-caption">
        <h3>Water Meter Cabinet</h3>
      </div>
    </div>
    <div class="item">
      <img src="{{ site.url }}/img/{{ page.id | remove_first:'/' | replace:'/','-' }}/3afde32399ed6507102ae11d55ce6aea.jpg" alt="Neptune R900 Wall MIU">
      <div class="carousel-caption">
	<h3>Neptune R900 Wall MIU</h3>
      </div>
    </div>
    <div class="item">
      <img src="{{ site.url }}/img/{{ page.id | remove_first:'/' | replace:'/','-' }}/6a43a1e97f88a9a4ac2ace448e4dd6d3.jpg" alt="Neptune T-10 Meter">
      <div class="carousel-caption">
        <h3>Neptune T-10 Meter <a href="{{ site.url }}/pdf/NeptunewT10s.pdf" target="_blank">(PDF)</a></h3>
      </div>
    </div>
  </div>

  <!-- Controls -->
  <a class="left carousel-control" href="#carousel-meter" role="button" data-slide="prev">
    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="right carousel-control" href="#carousel-meter" role="button" data-slide="next">
    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>

### The Dilemna
<p>
My first thought was to possibly capture the transmissions used for automatic meter reading with an SDR (software-defined radio).
Unfortunately, after looking into it a bit, it looked like by default the meter only reported every hour.
In addition, it looked like there was some non-trivial encoding being done on the signal, which would have made it a lot more work than I was hoping for.
</p>

<p>
Next up, I knew some power meters have an IR LED to report usage; however, I couldn't find one anywhere on the meter.
This left me with adding another meter in-line after the water company's, but most of the ones I could find were upwards of $200.
Since I was hoping to keep my budget under $50, this was a non-starter.
</p>

### The Idea
<p>
Rather stymied at this point, I began researching how water meters actually, well…meter.
Looking through the PDF linked in the image above, I discovered my meter is of the magnetic nutating disc type.
More or less, a disc wobbles as water is used which in turn spins a permanent magnet.
This oscillating magnetic field is picked up with a <a href="https://en.wikipedia.org/wiki/Hall_effect_sensor" target="_blank">Hall effect sensor</a>.
</p>

<p>
Now we're getting somewhere. It just so happened I had some spare parts from building a quadcopter with my friend including…a magnetometer.
Enter the magical <a href="https://www.sparkfun.com/products/10530" target="_blank">HMC5883L</a>.
With my handy Arduino, I wrote a quick sketch to stream the magnetometer's readings and stuck it onto the water meter. <strong>SUCCESS!</strong>
</p>

<img src="{{ site.url }}/img/{{ page.id | remove_first:'/' | replace:'/','-' }}/08d3ba297adada7826ec634107564d86.jpg" alt="HMC5883L">

### The Solution
<p>
At this point, all I had was a stream of field strengths in LSb (with Gain=1, 1090 LSb = 1 Gauss), but it did oscillate with water usage so I knew I had it.
The faster the oscillation, the more water being used.
After collecting the data for a day, the numbers seemed to bottom out at -928 LSb (-853.76 mG) and top out at -328 LSb (-301.76 mG).
To make things easier, I mapped that to -300 LSb and 300 LSb.
Lastly, I grabbed a 1L bucket and counted how many oscillations it took to fill it. This gave me an oscillation/liter ratio, which I could use to convert all future data into liters or gallons.
</p>

<p>
Skip forward a year and my <a href="https://www.kickstarter.com/projects/sparkdevices/spark-core-wi-fi-for-everything-arduino-compatible" target="_blank">Spark Core</a> arrived.
Time to add Wi-Fi!
Sadly, the first few months of the Spark Core were a little…rough, especially when trying to combine it with MQTT.
Short on time, I had to shelve the project. :[
</p>

<p>
<em>Let's do the time warp again.</em>
The year is 2015 and the successor to the Spark Core, the <a href="https://store.particle.io/?product=particle-photon" target="_blank">Particle Photon</a> is sitting on my front porch, waiting to change the world of water metering forever!
Okay, I might be overdoing it a little, but I was excited.
</p>

<div id="carousel-photon" class="carousel slide" data-ride="carousel">
  <!-- Indicators -->
  <ol class="carousel-indicators">
    <li data-target="#carousel-photon" data-slide-to="0" class="active"></li>
    <li data-target="#carousel-photon" data-slide-to="1"></li>
  </ol>

  <!-- Wrapper for slides -->
  <div class="carousel-inner" role="listbox">
    <div class="item active">
      <img src="{{ site.url }}/img/{{ page.id | remove_first:'/' | replace:'/','-' }}/013ad086ffa4a20971f5288beaa5bb33.jpg" alt="Final Setup">
      <div class="carousel-caption">
        <h3>The Final Setup</h3>
      </div>
    </div>
    <div class="item">
      <img src="{{ site.url }}/img/{{ page.id | remove_first:'/' | replace:'/','-' }}/22e7780a30863a27424c39e4e516b756.png" alt="Photon Schematic">
      <div class="carousel-caption">
        <h3>Photon Schematic</h3>
      </div>
    </div>
  </div>

  <!-- Controls -->
  <a class="left carousel-control" href="#carousel-photon" role="button" data-slide="prev">
    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="right carousel-control" href="#carousel-photon" role="button" data-slide="next">
    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>

### The Design
<p>
The Spark Core and the Photon are supposed to be interchangable so I dug out the old circuit, swapped the two chips, and uploaded the old code.
The good people at Particle didn't lie. Things worked and I had a way to monitor the data, but I needed a way to collect and store it.
I'd been looking into time series databases over the summer, and I was eager to try one of them out, meet <a href="https://influxdb.com/" target="_blank">InfluxDB</a>.
</p>

<p>
I figured I'd do something along the lines of having the Photon read the data, send it to some collection node with MQTT, and then store it in InfluxDB.
The Spark Core and Photon still seem to struggle with MQTT though so I opted to instead just skip that step entirely.
InfluxDB has an HTTP API, and the Photon can make HTTP requests, so I just had to combine the two, and the rest is history.
It also conveniently interfaces extremely well with <a href="http://grafana.org/" target="_blank">Grafana</a>.
</p>

<h3 id="the-code">The Code <a href="https://github.com/Shadow6363/Water-Meter-Reader" target="_blank"><i class="fa fa-fw fa-github"></i></a></h3>
{% highlight c++ %}
#define db_host "0.0.0.0"
#define db_port 0000
#define db_user ""
#define db_pass ""

#define hmc5883l_address 0x1E
#define publish_delay 10000


STARTUP(WiFi.selectAntenna(ANT_EXTERNAL));


unsigned long last_publish = 0;
unsigned long now = 0;
unsigned int crossings = 0;

int new_val = 0;
int old_val = 0;

boolean changed = false;

TCPClient client;
int16_t y;


void setup() {
    Wire.begin();

    Wire.beginTransmission(hmc5883l_address);
    Wire.write(0x00); // Select Configuration Register A
    Wire.write(0x38); // 2 Averaged Samples at 75Hz
    Wire.endTransmission();

    Wire.beginTransmission(hmc5883l_address);
    Wire.write(0x01); // Select Configuration Register B
    Wire.write(0x20); // Set Default Gain
    Wire.endTransmission();

    Wire.beginTransmission(hmc5883l_address);
    Wire.write(0x02); // Select Mode Register
    Wire.write(0x00); // Continuous Measurement Mode
    Wire.endTransmission();
}

void loop() {
    now = millis();

    Wire.beginTransmission(hmc5883l_address);
    Wire.write(0x03); // Select register 3, X MSB Register
    Wire.endTransmission();
    
    Wire.requestFrom(hmc5883l_address, 6); delay(4);
    if(Wire.available() >= 6) {
        // Ignore X and Z Registers
        Wire.read(); Wire.read(); Wire.read(); Wire.read();
        y  = Wire.read() << 8; // Y MSB
        y |= Wire.read();      // Y LSB
    }
    
    old_val = new_val;
    new_val = map(y, -928, -328, -300, 300);
    changed = (old_val < 0 && new_val > 0) || (old_val > 0 && new_val < 0);
    
    if(changed) {
        crossings += 1;
    }

    if(crossings > 0 && (now - last_publish) >= publish_delay) {
        if(client.connected() || client.connect(db_host, db_port)) {
            String influx_data =
                String::format("water value=%u", crossings);
            String http_content_len =
                String::format("Content-Length: %u", strlen(influx_data));
            String http_host =
                String::format("Host: %s:%u", db_host, db_port);
            String http_request =
                String::format("POST /write?db=meters&u=%s&p=%s HTTP/1.1",
                                db_user, db_pass);

            client.println(http_request);
            client.println(http_host);
            client.println(http_content_len);
            client.println();
            client.println(influx_data);
            client.println();
            client.flush();
            client.stop();
            
            crossings = 0;
            last_publish = now;
        }
    }
    
    delay(10);
}

{% endhighlight %}

### The Graph
<style>
.embed-container {
  position: relative;
  padding-bottom: 56.25%;
  height: 0;
  overflow: hidden;
  max-width: 100%;
}

.embed-container iframe, .embed-container object, .embed-container embed {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
<div class='embed-container'>
  <iframe src='http://tsdb.seductiveequations.com:3000/dashboard-solo/db/meters?panelId=1&fullscreen'></iframe>
</div>
