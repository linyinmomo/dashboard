#dashboard
#cctv.erb

    <script type='text/javascript'>
    $(function() {
    // These settings override the defaults set in application.coffee. You can do this on a per dashboard basis.
    Dashing.widget_base_dimensions = [370, 340]
    Dashing.numColumns = 5
        });
    </script>
    <% content_for :title do %>1080p dashboard<% end %
    <div class="gridster">
    <ul>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
        <div data-id="shanghai" data-view="Worldclock" data-timezone="Asia/Shanghai" data-title="Shanghai"     style="background-color:#667777;"></div>
        <i class="icon-time icon-background"></i>
    </li>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
      <div data-id="klimato" data-view="Klimato"></div>
    </li>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
      <div data-id="stockholm_weather" data-view="Klimato"></div>
    </li>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
      <div data-id="parkingspace" data-view="Number" data-title="Parking" ></div>
    </li>
       <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
        <div data-id="yahoo_stock_quote_eric_b_st" data-view="Enhancednumber" data-title="Ericsson B" style="background-color:#666666;" data-prefix="$"></div>
    </li>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
      <div data-id="aqi" data-view="Number" data-title="Air Quality" ></div>
    </li>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
        <div data-id="job1" data-view="JenkinsBuild" data-title="CBA CI" data-description=" " data-min="0" data-max="100"></div>
    </li>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
       <div data-id="lsv" data-view="Background" data-unordered="true" data-title="LSV"></div>
    </li>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
      <div data-id="sbg" data-view="Yanse" data-unordered="true" data-title="SBG" style="background-color: #9f516b" ></div>
    </li>
    <li data-row="2" data-col="2" data-sizex="1" data-sizey="2">
      <div data-id="pie" data-view="Pie" data-legend="true" data-title="TR Registered From" style="background-color: #A0A0A0"></div>
    </li>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
      <div data-id="dailyone" data-view="Yanse" data-unordered="true" data-title="Daily Test1" style="background-color: #0f516b" data-moreinfo="Value in USD"></div>
    </li>
    </li>
    <li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
      <div data-id="daily" data-view="Yanse" data-unordered="true" data-title="Daily Text2" style="background-color: #0f516b" data-moreinfo="Value in USD"></div>
    </li>
    <li data-row="1" data-col="1" data-sizex="2" data-sizey="1">
      <div data-id="convergence" data-view="Rickshawgraph" data-title="SBG TR INFLOW" data-renderer="line" data-legend="true" style="background-color:#505050"></div>
    </li>
    </ul>
    </div>
 

#daily1.rb

    require 'open-uri'
    require "net/http"
    require 'nokogiri'
    require 'rubygems'


    SCHEDULER.every '1h', :first_in => 0 do |job|
    url = 'link'
    @doc = Nokogiri::HTML(open(url))
    list = [25,30,35,40,45,50,55]
    list2 = [26,31,36,41,46,51,56]
    name1 = []
    name2 = []
    select = []
    number = []
    title = "LLV Simulated"
    moreinfo = @doc.xpath("//td")[13].text
    list.each do |a_tag|
    name1 << @doc.xpath("//td")[a_tag].text
    end  
    name1.each do |a_tag|
    if a_tag[0] == "["
        name2 << a_tag.split("[")[1].split("]")[0]
    else
        name2 << a_tag
    end
    end
    state = []
    level = [] 
    color = []
    list.each do |a_tag|
    color << @doc.xpath("//td")[a_tag].attribute('bgcolor').to_s
    end
    color.each do |a|
    case a.upcase
    when "#FFFFFF" then   #white
    state << "N/A"
    when "#FF0000" then  #red
        state << "Failed"
    when "#696969" then #black unstable numbver
        state << "Aborted"
    when "#FF9900" then    #yellow
        state << "ENV-err"
    when "#98FB98" then  #green successful number
        state << "Successful"
    when "#FFFF00" then 
        state << "High"
    else
    state << "Running"   #blue
    end
    end
    state.each do |a_tag|
    case a_tag
    when "Running" then #blue
        level << "value-3"
    when "Failed" then  #red
        level << "value-1"
    when "Aborted" then  #red
        level << "value-1"
    when "N/A" then  #red
        level << "value-1"
    when "Successful" then  #green successful number
        level << "value-2"  
    when "ENV-err" then  #yellow
        level << "value-0"
    when "High" then
        level << "value-7"
    end 
    end
    list2.each do |a_tag|
    select << @doc.xpath("//td")[a_tag].text
    end 
    (0..select.length-1).each do |k|
    if select[k] == "\n"
        number << state[k]
    else 
        a_tag = select[k]
     if a_tag.length <= 4
       number << a_tag.gsub("\n", "%")
     else    
       number << a_tag.split('(')[1].split(')')[0] unless a_tag.empty?
    end      
    end
    end 
    stocklist = Array.new
    (0..name2.length-1).each do |a|
        stocklist.push({
        label: name2[a],
        value: number[a],
        level: level[a]
      })
    end
    if defined?(send_event)
      send_event('dailyone', { title: title, moreinfo: moreinfo, items: stocklist })
    else
      print stocklist
    end
    end

#daily2.rb

    #!/usr/bin/env ruby
    require 'open-uri'
    require "net/http"
    require 'nokogiri'
    require 'rubygems'


    SCHEDULER.every '1h', :first_in => 0 do |job|
    url = 'https://ims-sbgweb.rnd.ki.sw.ericsson.se/wiki/Daily_test_status'
    @doc = Nokogiri::HTML(open(url))
    list = [65,69,73,77,81,85]
    list2 = [66,70,74,78,82,86]
    name1 = []
    name2 = []
    number = []
    select = []
    #tit1 = @doc.xpath("//td")[17].text
    #tit2 = @doc.xpath("//td")[18].text
    #title = tit1 + tit2
    title = "LLV Target"
    moreinfo = @doc.xpath("//td")[13].text
    #name
    list.each do |a_tag|
    name1 << @doc.xpath('//td')[a_tag].text
    end
    name1.each do |a_tag|
    if a_tag[0] == "["
        name2 << a_tag.split("[")[1].split("]")[0]
    else
        name2 << a_tag
    end
    end 
    #color
    state = []
    level = [] 
    color = []
    list.each do |a_tag|
    color << @doc.xpath("//td")[a_tag].attribute('bgcolor').to_s
    end
    color.each do |a|
    case a.upcase
    when "#FFFFFF" then   #white
        state << "N/A"
    when "#FF0000" then  #red
        state << "Failed"
    when "#696969" then #black unstable numbver
        state << "Aborted"
    when "#FF9900" then    #yellow
        state << "ENV-err"
    when "#98FB98" then  #green successful number
        state << "Successful"
    when "#FFFF00" then 
        state << "High"
    else
        state << "Running"   #blue
    end
    end
    state.each do |a_tag|
    case a_tag
    when "Running" then #blue
        level << "value-3"
    when "Failed" then  #red
        level << "value-1"
    when "Aborted" then  #red
        level << "value-1"
    when "N/A" then  #red
            level << "value-1"
        when "Successful" then  #green successful number
        level << "value-2"  
    when "ENV-err" then  #yellow
        level << "value-0"
    when "High" then
        level << "value-7"
    end 
    end
    list2.each do |a_tag|
    select << @doc.xpath("//td")[a_tag].text
    end 
    (0..select.length-1).each do |k|
    if select[k] == "\n"
        number << state[k]
    else 
        a_tag = select[k]
        if a_tag.length <= 4
        number << a_tag.gsub("\n", "%")
        else    
        number << a_tag.split('(')[1].split(')')[0] unless a_tag.empty?
    end      
    end
    end
    #push
    stocklist = Array.new
    (0..name2.length-1).each do |a|
        stocklist.push({
            label: name2[a],
            value: number[a],
            level: level[a]
        })
    end
    if defined?(send_event)
      send_event('daily', { title: title, moreinfo: moreinfo, items: stocklist })
    else
      print stocklist
    end
    end
      
#line.rb

     #!/usr/bin/env ruby
    require 'open-uri'
    require "net/http"
    require 'nokogiri'
    require 'json'
    require 'rubygems'
    SCHEDULER.every '360h', :first_in => 0 do |job|
    #@doc = Nokogiri::HTML(File.open("SBG (CBA) Passrate Radiator.htm"))
    url = 'http://137.58.131.87:8000/charts/halfyearly/all/2014-9'
    @doc = Nokogiri::HTML(open(url))
    #a = @doc.xpath("//script").grep(/DataTable\((.*), 0.6\);\n/){$1}
    #a = @doc.xpath("//script").grep(/DataTable\((.*), 0.6\);\n/){$1}
    text = @doc.xpath("//script")
    script = text.to_html
    #first table
    data = script.split("json_data = new google.visualization.DataTable(")[2]
    json_data = data.split(", 0.6);\n")[0]
    hash = JSON.parse(json_data)
    #name 
    name = []
    hash["cols"].each do |a|
    name << a["id"]
    end
    #x values
    hash1 = {}
    number = [] # all values
    x1_n = []
    hash["rows"].each do |a|
    hash1 = a["c"]
    hash1.each do |b|
    if b["v"].class == String
        x1_n << number.length
    number << b["v"]
    else
        number<< b["v"]
    end
    end
    end
    x1 = []
    x1_n.each do |k|
    x1 << number[k]
    end
    x = []
    x1.each do |a| 
    case a
    when "JAN" then
        x <<  1388534400
    when "FEB" then
        x << 1391212800
    when "MAR" then
        x << 1393632000
    when "APR" then
        x << 1396310400
    when "MAY" then
        x << 1398902400
    when "JUN" then
        x << 1401580800
    when "JUL" then
        x << 1404172800
    when "AUG" then
        x << 1406851200
    when "SEP" then
        x << 1409529600
    when "OCT" then
        x << 1412121600  
    when "NOV" then
        x << 1414800000  
    when "DEC" then
        x << 1417392000
    end
    end
    colors = ["#000000", "#003300", "#006600", "#330000", "#ff0000", "#00ff00", "#0000ff", "#00ffff", "#ff00ff", "#000066", "#996666", "#00CC99", "#660033", "#cc0000", "#999ff00", "#cc6600", "#cc0066", "#9900ff", "#ff99cc", "#66ff00"]
    # y values
    y = []
    (1..name.length-1).each do |k|
    x1_n.each do |a|
    y << number[a+k]
    end
    end
    #data push
    data_push = []
    data1 = []
    #count = 0
    (0..name.length-2).each do |k|
    (0..x.length-1).each do |a|
        data_push << {x: x[a], y: y[a+6*k]}
        if a % 6 == 5
        data1 << data_push.to_json
    end 
    end
    data_push = []
    end
    series = Array.new
    (0..name.length-2).each do |a_tag|
    series.push({
            name: name[a_tag+1],
        color: colors[a_tag],
        data: JSON.parse(data1[a_tag])       
      })
    end
    if defined?(send_event)
        send_event('convergence', series: series, value: y.max)
    else
        print series
    end
    end
#lsv.rb

    #!/usr/bin/env ruby
    require 'open-uri'
    require "net/http"
    require 'nokogiri'
    require 'rubygems'
    SCHEDULER.every '10m', :first_in => 0 do |job|
    #@doc = Nokogiri::HTML(File.open("LSV status - Brain.mht"))
    url = 'https://ims-sbgweb.rnd.ki.sw.ericsson.se/wiki/LSV_status'
    @doc = Nokogiri::HTML(open(url))
    title =  @doc.xpath("//td")[6].text
    moreinfo = "P/OK(OK to Deliver)"
    #name
    name = []
    (2..7).each do |a|
    if @doc.xpath("//th")[a].text =~ /,/
    name << @doc.xpath("//th")[a].text.split(",")[0] 
    elsif @doc.xpath("//th")[a].text =~ /\(/
    if @doc.xpath("//th")[a].text.split(")")[1].length < 6
        name << @doc.xpath("//th")[a].text.split("(")[0] + @doc.xpath("//th")[a].text.split(")")[1]
    else
        name << @doc.xpath("//th")[a].text.split("(")[0]
    end
    else 
    name << @doc.xpath("//th")[a].text
    end
    end
    #color
    color = []
    [7,8,9,10].each do |b|
    if @doc.xpath("//td")[b].attribute('bgcolor') == nil
    color << @doc.xpath("//tr")[9].attribute('bgcolor').to_s
    else
    color << @doc.xpath("//td")[b].attribute('bgcolor').to_s
    end
    end
    [10,11].each do |c|
    if @doc.xpath("//tr")[c].attribute('bgcolor') == nil
    color << @doc.xpath("//td")[11].attribute('bgcolor').to_s
    else 
    color << @doc.xpath("//tr")[c].attribute('bgcolor').to_s
    end
        end
    #level
    level = []
    color.each do |d|
    if d =~ /ffff/  
    level << "value-3"
    elsif d =~ /99dd/
    level << "value-2" 
    elsif d =~ /ff99/
    level << "value-4"
    elsif d =~ /pink/
    level << "value-4"
    elsif d =~ /dddd/
    level << "value-6"
    elsif d=~ /white/
    level << "value-5"
    else
    level << "value-5"
    end
    end
    #state
    state = []
    level1 = []
    level.each do |f|
    case f
    when "value-3" then
    state << "P/OK"
    level1 << "value-3"
    when "value-2" then
    state << "OK"
    level1 << "value-3"
    when "value-4" then
    state << "NOK"
    level1 << "value-1"
    when "value-5" then
    state << "Not on time"
    level1 << "value-1"
    when "value-6" then
    state << "Not planned"
    level1 << "value-1"
    end
    end
    stocklist = Array.new
    (0..name.length-1).each do |a_tag|
    stocklist.push({
            label: name[a_tag],
            value: state[a_tag],
            level: level1[a_tag]
        })
    end
    if defined?(send_event)
        send_event"lsv", { title: title, moreinfo: moreinfo, background: color[0], items: stocklist }
    else
        print stocklist
    end
    end
    

#pie.rb
   

      #!/usr/bin/env ruby
    require 'open-uri'
    require "net/http"
    require 'nokogiri'
    require 'json'
    require 'rubygems'
    SCHEDULER.every '360h', :first_in => 0 do |job|
    #@doc = Nokogiri::HTML(File.open("SBG TR Statistics - Yearly.htm"))
    url = 'http://137.58.131.87:8000/charts/halfyearly/all'
    @doc = Nokogiri::HTML(open(url))
    text = @doc.xpath("//script")
    script = text.to_html
    #first table
    data = script.split("json_data = new google.visualization.DataTable(")[5]
    json_data = data.split(", 0.6);\n")[0]
    hash = JSON.parse(json_data  
    #x values
    hash1 = {}
    number = [] # all values
    x1_n = []
    hash["rows"].each do |a|
    hash1 = a["c"]
    hash1.each do |b|
    if b["v"].class == String
        x1_n << number.length
    number << b["v"]
    else
        number<< b["v"]
    end
    end
    end
    #name 
    name = []
    x1_n.each do |k|
    name << number[k]
    end
    value = []
    x1_n.each do |k|
    value << number[k+1]
    end 
    series = Array.new
    (0..name.length-1).each do |a_tag|
    series.push({
            label: name[a_tag],
            value: value[a_tag]       
        })
    end
    if defined?(send_event)
        #send_event('convergence1', series: series)
        send_event 'pie', { value: series }
    else
      print series
    end
    end
