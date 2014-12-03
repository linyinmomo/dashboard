#cctv.erb


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

#line.rb

#lsv.rb

#parking.rb

#pie.rb

#sbg.rb

#stock.rb
