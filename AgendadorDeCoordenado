// ==UserScript==
// @name           Agendador de coordenado
// @author         Ericson Bizarro
// @email          ericsonbizarro@gmail.com
// @version        1.0
// @grant          Publico
// @description    Agendador de ataque  (linguagem: javascript-ECMAscript5;)
// @Utilização     NA PRAÇA DE REUNIÃO OPÇÃO CONFIRMAR ATAQUE (ULTIMA ETAPA DE UM ATAQUE)
// @include        https://*screen=place&try=confirm*
// @require https://code.jquery.com/jquery-2.2.4.min.js
// @run-at         document-start
// @icon           https://media.innogamescdn.com/com_DS_FR/Quickbar/priest.png
// ==/UserScript==

var CommandSender = {
 confirmButton: null,
 duration: null,
 dateNow: null,
 offset: null,
    init: function() {
  $($('#command-data-form').find('tbody')[0]).append('<tr><td>Chegada:</td><td> <input type="datetime-local" id="CStime" step=".001"> </td></tr><tr> <td>Offset:</td><td> <input type="number" id="CSoffset"> <button type="button" id="CSbutton" class="btn">Confirmar</button> </td></tr>');
  this.confirmButton = $('#troop_confirm_go');
  this.duration = $('#command-data-form').find('td:contains("Duração:")').next().text().split(':').map(Number);
  this.offset = localStorage.getItem('CS.offset') || -250;
  this.dateNow = this.convertToInput(new Date());
  $('#CSoffset').val(this.offset);
  $('#CStime').val(this.dateNow);
  $('#CSbutton').click(function() {
   var offset = Number($('#CSoffset').val());
   var attackTime = CommandSender.getAttackTime();
   localStorage.setItem('CS.offset', offset);
   CommandSender.confirmButton.addClass('btn-disabled');
   setTimeout(function() {
    CommandSender.confirmButton.click();
   },attackTime-Timing.getCurrentServerTime()+offset);
   this.disabled = true;
  });
 },
 getAttackTime: function() {
  var d = new Date($('#CStime').val().replace('T',' '));
  d.setHours(d.getHours()-this.duration[0]);
  d.setMinutes(d.getMinutes()-this.duration[1]);
  d.setSeconds(d.getSeconds()-this.duration[2]);
  return d;
 },
 convertToInput: function(t) {
  t.setHours(t.getHours()+this.duration[0]);
  t.setMinutes(t.getMinutes()+this.duration[1]);
  t.setSeconds(t.getSeconds()+this.duration[2]);
  var a = {
   y: t.getFullYear(),
   m: t.getMonth() + 1,
   d: t.getDate(),
   time: t.toTimeString().split(' ')[0],
   ms: t.getMilliseconds()
  };
  if (a.m < 10) {
   a.m = '0' + a.m;
  }
  if (a.d < 10) {
   a.d = '0' + a.d;
  }
  if (a.ms < 100) {
   a.ms = '0' + a.ms;
   if (a.ms < 10) {
    a.ms = '0' + a.ms;
   }
  }
  return a.y + '-' + a.m + '-' + a.d + 'T' + a.time + '.' + a.ms;
 },
 addGlobalStyle: function(css) {
  var head, style;
     head = document.getElementsByTagName('head')[0];
     if (!head) { return; }
     style = document.createElement('style');
     style.type = 'text/css';
     style.innerHTML = css;
     head.appendChild(style);
 }
};
CommandSender.addGlobalStyle('#CStime, #CSoffset {font-size: 9pt;font-family: Verdana,Arial;}#CSbutton {float:right;}');
var a = setInterval(function(){
 if (document.getElementById('command-data-form') && jQuery) {
  CommandSender.init();
  clearInterval(a);
 }
},1); // faster load
