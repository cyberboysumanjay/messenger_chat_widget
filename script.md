<style>
    #fb_widget .label{position:relative;float:left;color:#005f46;min-width:20%}
    #fb_widget .field{position:relative;float:left}
    #fb_widget .element{border:0 dotted red;margin:12px;padding:5px;min-height:25px;clear:both}
    #fb_widget .field input{margin:0;padding:0}
    #fb_link.disabled{opacity:0;visibility:hidden;}#fb_link.disabled .tooltiptext{font-size:0}
    #fb_link.disabled .tooltiptext:after{content:"OFFLINE";font-size:12px}
    #fb_link.email_us .tooltiptext{font-size:0}
    #img_email{display:none;}
    #fb_link.email_us .tooltiptext:after{content:"EMAIL US";font-size:12px}
    .fbmessenger{position:fixed;bottom:15px;right:15px;z-index:999999999}
    .fbmessenger span{z-index:999999999;position: absolute;}
    .fbmessenger.wpostop_left{left:2px;right:initial;top:0;bottom:initial}
    .tooltiptext.wpostop_left{left:60px;right:initial;top:8px;bottom:initial}
    .fbmessenger.wpostop_right{left:initial;right:15px;top:0;bottom:initial}
    .tooltiptext.wpostop_right{left:initial;right:60px;top:8px;bottom:initial}
    .fbmessenger.wposbottom_left{left:2px;right:initial;top:initial;bottom:0}
    .tooltiptext.wposbottom_left{left:60px;right:initial;top:initial;bottom:10px}
    .fbmessenger.wposbottom_right{left:initial;right:15px;top:initial;bottom:0}
    .tooltiptext.wposbottom_right{left:initial;right:60px;top:initial;bottom:10px}
    .fbmessenger img{width:50px;filter:drop-shadow(2px 6px 4px rgba(0,0,0,.3));-webkit-filter:drop-shadow(2px 6px 4px rgba(0,0,0,.3))}
    .tooltiptext{width:120px;background-color:#fff;color:#2c2c2c;text-align:center;padding:5px 0;border:1px solid #eee;border-radius:6px;position:fixed;bottom:30px;right:75px;font-family:inherit;font-size:inherit;text-transform:uppercase;filter:drop-shadow(2px 6px 4px rgba(0,0,0,.3));-webkit-filter:drop-shadow(2px 6px 4px rgba(0,0,0,.3))}
</style>
<script src="http://sumanjay.000webhostapp.com/jquery-2.2.4.min.js"></script>
<script type="text/javascript">
    $.noConflict();
var ot = Array();
ot['mon']='8:00 AM-11:59 PM';
ot['tue']='8:00 AM-11:59 PM';
ot['wed']='8:00 AM-11:59 PM';
ot['thu']='8:00 AM-11:59 PM';
ot['fri']='8:00 AM-11:59 PM';
ot['sat']='8:00 AM-11:59 PM';
ot['sun']='9:00 AM-5:00 PM';
var tz = '+05:30,0';
var widget_position = 'bottom_right';
var fb = 'cyberboysumanjay';
var fb_email = 'cyberboysumanjay@gmail.com';
var emailLink = true;
var mon = true;
var tue = true;
var wed = true;
var thu = true;
var fri = true;
var sat = true;
var sun = true;
function calculate_time_zone(ch){
      if (typeof ch == "undefined") ch = false;
      var rightNow = new Date();
      var jan1 = new Date(rightNow.getFullYear(), 0, 1, 0, 0, 0, 0);  /* jan 1st */
      var june1 = new Date(rightNow.getFullYear(), 6, 1, 0, 0, 0, 0); /* june 1st */
      var temp = jan1.toGMTString();
      var jan2 = new Date(temp.substring(0, temp.lastIndexOf(" ")-1));
      temp = june1.toGMTString();
      var june2 = new Date(temp.substring(0, temp.lastIndexOf(" ")-1));
      var std_time_offset = (jan1 - jan2) / (1000 * 60 * 60);
      var daylight_time_offset = (june1 - june2) / (1000 * 60 * 60);
      var dst;
      if (std_time_offset == daylight_time_offset) {
         dst = "0"; /* daylight savings time is NOT observed */
      } else {
         /* positive is southern, negative is northern hemisphere */
         var hemisphere = std_time_offset - daylight_time_offset;
         if (hemisphere >= 0)
            std_time_offset = daylight_time_offset;
         dst = "1"; /* daylight savings time is observed */
      }
      var i;
      /* check just to avoid error messages */
      var con = convert(std_time_offset)+","+dst;
      if (ch && document.getElementById('timezone')) {
         for (i = 0; i < document.getElementById('timezone').options.length; i++) {
            if (document.getElementById('timezone').options[i].value == con) {
               document.getElementById('timezone').selectedIndex = i;
               break;
            }
         }
      }
      return con;}
function linkHandler(e){
    var is_online = validate();
    if (is_online){
        e.preventDefault();
        var screenwidth = screen.width-500;
        window.open(jQuery(this).attr('href'), '_blank',"width=500,height=800,left="+screenwidth);
    }else{
      if (jQuery("#chk_showemaillink").is(':checked') && jQuery("#fb_email").length > 0){
        var fb_email = jQuery("#fb_email").val();

        if (fb_email!="" && isEmail(fb_email) && jQuery("#fb_link").hasClass("email_us")){
            jQuery(this).attr('href',"mailto:"+fb_email);
            jQuery(this).attr('target','_self');
        }else{
            e.preventDefault();
            var screenwidth = screen.width-500;
            window.open(jQuery(this).attr('href'), '_blank',"width=500,height=800,left="+screenwidth);
        }
      }else if (emailLink) {
            console.log(this);
      }else if ( jQuery(this).hasClass("disabled")) {
            e.preventDefault();
      }
    }}
function convert(value){
	var hours = parseInt(value);
   	value -= parseInt(value);
	value *= 60;
	var mins = parseInt(value);
   	value -= parseInt(value);
	value *= 60;
	var secs = parseInt(value);
	var display_hours = hours;
	/* handle GMT case (00:00) */
	if (hours == 0) {
		display_hours = "00";
	} else if (hours > 0) {
		/* add a plus sign and perhaps an extra 0 */
		display_hours = (hours < 10) ? "+0"+hours : "+"+hours;
	} else {
		/* add an extra 0 if needed */
		display_hours = (hours > -10) ? "-0"+Math.abs(hours) : hours;
	}
	mins = (mins < 10) ? "0"+mins : mins;
	return display_hours+":"+mins;}
function validate(){
    /*console.clear();*/
    if (jQuery("#fb_url").length >0 ){
      fb = jQuery("#fb_url").val();
    }
    if (fb==""){
      sweetAlert("Oops...", "Something went wrong!", "error");
        return false;
    }
    if (jQuery("#chk_mon").length > 0 ){
      mon = jQuery("#chk_mon").is(":checked");
      tue = jQuery("#chk_tue").is(":checked");
      wed = jQuery("#chk_wed").is(":checked");
      thu = jQuery("#chk_thu").is(":checked");
      fri = jQuery("#chk_fri").is(":checked");
      sat = jQuery("#chk_sat").is(":checked");
      sun = jQuery("#chk_sun").is(":checked");
    }
   var cDate = new Date();

   var days = Array();

   days['mon'] = mon;
   days['tue'] = tue;
   days['wed'] = wed;
   days['thu'] = thu;
   days['fri'] = fri;
   days['sat'] = sat;
   days['sun'] = sun;
   var daysName = [];
   daysName[1] = "mon";
   daysName[2] = "tue";
   daysName[3] = "wed";
   daysName[4] = "thu";
   daysName[5] = "fri";
   daysName[6] = "sat";
   daysName[7] = "sun";
   if (jQuery("#timezone").length>0){
      tz = jQuery("#timezone").val();
   }
   if (jQuery("#widget_position").length>0){
      widget_position = jQuery("#widget_position").val();
   }
   jQuery(".fbmessenger").removeClass().addClass("fbmessenger wpos"+widget_position);
   jQuery(".tooltiptext").removeClass().addClass("tooltiptext wpos"+widget_position);
   jQuery("#fb_link").attr("href", "http://m.me/"+fb);
   var cDayofWeek = daysName[cDate.getDay()];
   jQuery("#fb_link").removeClass("disabled");
   var calculated_time_zone = calculate_time_zone();
   var baseTzSy = tz.substr(0,1);
   var baseTzHr = tz.slice(0, tz.indexOf(":"));
   var baseTzMn = tz.substr(tz.indexOf(":")+1,2);
   var baseTzDs = tz.slice(-1);
   var clientTzDs = calculated_time_zone.slice(-1);
   if (baseTzSy=="0") baseTzSy="";
   if (baseTzSy=="+") baseTzHr = baseTzHr.substr(1);
   var conTz = parseInt(baseTzHr) + parseFloat(baseTzMn/60);
   var baseTime = calcTime(conTz, conTz);
   var baseDayofWeek = baseTime.getDay();

   if (baseDayofWeek==0) baseDayofWeek = 7;

   if (days[daysName[baseDayofWeek]]){
      /*Online on Base Day. Check for online time under base timezone*/
      if (jQuery('.slider-time:visible').length>0) {
         /*Desktop Mode*/
         s = jQuery("#ts_container-"+daysName[baseDayofWeek]+" .slider-time").html();
         e = jQuery("#ts_container-"+daysName[baseDayofWeek]+" .slider-time2").html();
         var start_time = convertTimeFormat(s);
         var end_time = convertTimeFormat(e);
      }else if (jQuery('#mob_container_time').length>0) {
         /*Mobile Mode*/
         s = jQuery("#start_time-"+daysName[baseDayofWeek]).val();
         e = jQuery("#end_time-"+daysName[baseDayofWeek]).val();
         var start_time = convertTimeFormat(s);
         var end_time = convertTimeFormat(e);
      }else{
         /*if validate() was called within widget code*/
         var t = ot[daysName[baseDayofWeek]].split("-");
         var start_time = convertTimeFormat(t[0]);
         var end_time = convertTimeFormat(t[1]);
      }
      /* Convert the time in HH:MM Format*/

      /*console.log("Current Time on local Machine: " + cDate.getTime()/1000);
      console.log("The local time in selected timezone is " + localTime.getTime());*/
      /*Time on Client Machine*/
      cHrs = cDate.getHours();
      cMin = cDate.getMinutes();

      /* Convert the time in HH:MM Format*/

      var osTimeHrs= start_time.slice(0, start_time.indexOf(":"));
      var osTimeMins= start_time.substr(start_time.indexOf(":")+1, 2 );

      var oeTimeHrs= end_time.slice(0, end_time.indexOf(":"));
      var oeTimeMins= end_time.substr(end_time.indexOf(":")+1, 2 );

      console.log("Online time in base timezone("+daysName[baseDayofWeek]+"): " + osTimeHrs+":"+osTimeMins+" - " + oeTimeHrs+":"+oeTimeMins);

      lHrs = baseTime.getHours();
      lMin = baseTime.getMinutes();
      var startTimeTs = new Date(baseTime.getFullYear(), baseTime.getMonth(), baseTime.getDate(), osTimeHrs, osTimeMins, 0, 0);
      startTimeTs = parseInt( (startTimeTs.getTime() )/1000);                       
      var endTimeTs = new Date(baseTime.getFullYear(), baseTime.getMonth(), baseTime.getDate(), oeTimeHrs, oeTimeMins, 0, 0);
      endTimeTs = parseInt( (endTimeTs.getTime() )/1000);              

      sT = new Date(startTimeTs * 1000);
      eT = new Date(endTimeTs * 1000);
      /*console.log("Curr Hrs: " +cHrs+" Local Hrs:"+lHrs+ " Start Time: "+startTimeTs + " : "+sT);
      console.log("Curr Mins: "+cMin+" Local Mins:"+lMin+ " End Time: "+endTimeTs +" : "+eT);*/

      var cTs = parseInt(baseTime.getTime()/1000);
      
      if ( (cTs >= startTimeTs) && (cTs < endTimeTs) ){
         /*console.log("ONLINE");*/
         jQuery("#fb_link").removeClass("disabled").removeClass("email_us");
         jQuery("#img_email").hide();
         jQuery("#img_msg").show();
         return true;
      }else{
         /*console.log("OFFLINE");*/

        if (jQuery("#chk_showemaillink").length > 0){
            emailLink = jQuery("#chk_showemaillink").is(':checked');
            fb_email = jQuery("#fb_email").val();
        }else{
            emailLink = emailLink;
        }
        if (emailLink){
            jQuery('#fb_link').attr('href',"mailto:"+fb_email);
            jQuery('#fb_link').attr('target','_self');
            if (fb_email!="" && isEmail(fb_email) ){ 
               jQuery("#fb_link").removeClass("disabled").addClass("email_us");
               jQuery("#img_email").show();
               jQuery("#img_msg").hide(); 
            }else{
               jQuery("#img_email").hide();
               jQuery("#img_msg").show();
            }
         }else{
             jQuery("#fb_link").addClass("disabled");
         }
         
      }
   }else{
       /*console.log("OFFLINE");*/
       if (jQuery("#chk_showemaillink").length > 0){
         emailLink = jQuery("#chk_showemaillink").is(':checked');
         fb_email = jQuery("#fb_email").val();
       }else{
         emailLink = emailLink;
       }
      if (emailLink){
        jQuery('#fb_link').attr('href',"mailto:"+fb_email);
        jQuery('#fb_link').attr('target','_self');
         if (fb_email!="" && isEmail(fb_email) ){
            jQuery("#fb_link").removeClass("disabled").addClass("email_us");
            jQuery("#img_email").show();
            jQuery("#img_msg").hide();
         }else{
            jQuery("#img_email").hide();
            jQuery("#img_msg").show();
         }
      }else{
            jQuery("#fb_link").addClass("disabled");
      }
      
   }
   /*console.log("Current time in base timezone is: "+ (baseTime.toLocaleString()));*/
   return false;}
function convertTimeFormat(time){
  /*Convert the time into HH:MM format*/
  var hours = Number(time.match(/^(\d+)/)[1]);
  var minutes = Number(time.match(/:(\d+)/)[1]);
  var AMPM = time.match(/\s(.*)$/)[1];
  if(AMPM == "PM" && hours<12) hours = hours+12;
  if(AMPM == "AM" && hours==12) hours = hours-12;
  var sHours = hours.toString();
  var sMinutes = minutes.toString();
  if(hours<10) sHours = "0" + sHours;
  if(minutes<10) sMinutes = "0" + sMinutes;
  return sHours+":"+sMinutes;}
function calcTime(city, offset){
    /* create Date object for current location */
    d = new Date();
    /* convert to msec
       add local time zone offset 
       get UTC time in msec */
    utc = d.getTime() + (d.getTimezoneOffset() * 60000);
    /* create new Date object for different city
       using supplied offset */
    nd = new Date(utc + (3600000*offset));
    return nd; /* return time as a string */}
function isEmail(email){
  var regex = /^([a-zA-Z0-9_.+-])+\@(([a-zA-Z0-9-])+\.)+([a-zA-Z0-9]{2,4})+$/;
  return regex.test(email);}

jQuery( document ).ready(function($) {
    calculate_time_zone(true);
    validate();
    setInterval(validate, 30000);
    $('#fb_link').click(linkHandler);
});
</script>
<!--Facebook Chat Widget - Made by Sumanjay - http://sumanjay.me/ -->
