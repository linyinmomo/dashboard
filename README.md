dashboard
=========

//cctv.erb
a beautiful dashboard
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
    <div data-id="shanghai" data-view="Worldclock" data-timezone="Asia/Shanghai" data-title="Shanghai" style="background-color:#667777;"></div>
    <i class="icon-time icon-background"></i>
  </li>
<!--
    <li data-row="1" data-col="1" data-sizex="2" data-sizey="1">
      <div data-id="welcome" data-view="Text" data-title="PDC Gateway Shanghai" data-text="SBG Development Information Board" data-moreinfo="Any suggestions, please contact Sean Yin"></div>
    </li>
-->
  
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
