---
layout: post
comments: true
title:  Kernel Density Estimation (커널 밀도 추정)
categories: Noverty-Detection

tags:
- Novelty Detection
- Density Estimation
---

**<span style='color:DarkRed'> Nonparametric Estimation </span>**


> - Nonparametric 추정방법은 모수를 가정하지 않고 분포를 예측하는 방법으로, **Training data**를 기반으로 좀 더 유연한(다양한) 분포를 형성할 수 있음

<table style="width:100%">
  <tr>
    <th></th>
    <th><span style='color:blue'>Gaussian distribution estimation</span></th> 
    <th><span style='color:green'>Parzen window estimation</span></th>
  </tr>
  <tr>
    <th>방법</th>
    <td> Parametric </td> 
    <td> Nonparametric</td>
  </tr>
  <tr>
    <th>사용된 모수</th>
    <td>$\mu$ (분포의 위치를 결정), $\sigma$ (분포의 모양)</td> 
    <td> - </td>
  </tr>
   <tr>
    <th> 분포의 다양성 </th>
    <td> smooth한 분포 표현 </td> 
    <td> flexible한 분포 표현 </td>
  </tr>
</table>


<html>
<head><meta charset="utf-8" />
<title>KDE</title><script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>

<style type="text/css">
    /*!
*
* Twitter Bootstrap
*
*/
/*!
 * Bootstrap v3.3.7 (http://getbootstrap.com)
 * Copyright 2011-2016 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE)
 */
/*! normalize.css v3.0.3 | MIT License | github.com/necolas/normalize.css */
html {
  font-family: sans-serif;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}
body {
  margin: 0;
}
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
main,
menu,
nav,
section,
summary {
  display: block;
}
audio,
canvas,
progress,
video {
  display: inline-block;
  vertical-align: baseline;
}
audio:not([controls]) {
  display: none;
  height: 0;
}
[hidden],
template {
  display: none;
}
a {
  background-color: transparent;
}
a:active,
a:hover {
  outline: 0;
}
abbr[title] {
  border-bottom: 1px dotted;
}
b,
strong {
  font-weight: bold;
}
dfn {
  font-style: italic;
}
h1 {
  font-size: 2em;
  margin: 0.67em 0;
}
mark {
  background: #ff0;
  color: #000;
}
small {
  font-size: 80%;
}
sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}
sup {
  top: -0.5em;
}
sub {
  bottom: -0.25em;
}
img {
  border: 0;
}
svg:not(:root) {
  overflow: hidden;
}
figure {
  margin: 1em 40px;
}
hr {
  box-sizing: content-box;
  height: 0;
}
pre {
  overflow: auto;
}
code,
kbd,
pre,
samp {
  font-family: monospace, monospace;
  font-size: 1em;
}
button,
input,
optgroup,
select,
textarea {
  color: inherit;
  font: inherit;
  margin: 0;
}
button {
  overflow: visible;
}
button,
select {
  text-transform: none;
}
button,
html input[type="button"],
input[type="reset"],
input[type="submit"] {
  -webkit-appearance: button;
  cursor: pointer;
}
button[disabled],
html input[disabled] {
  cursor: default;
}
button::-moz-focus-inner,
input::-moz-focus-inner {
  border: 0;
  padding: 0;
}
input {
  line-height: normal;
}
input[type="checkbox"],
input[type="radio"] {
  box-sizing: border-box;
  padding: 0;
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: textfield;
  box-sizing: content-box;
}
input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none;
}
fieldset {
  border: 1px solid #c0c0c0;
  margin: 0 2px;
  padding: 0.35em 0.625em 0.75em;
}
legend {
  border: 0;
  padding: 0;
}
textarea {
  overflow: auto;
}
optgroup {
  font-weight: bold;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
td,
th {
  padding: 0;
}
/*! Source: https://github.com/h5bp/html5-boilerplate/blob/master/src/css/main.css */
@media print {
  *,
  *:before,
  *:after {
    background: transparent !important;
    color: #000 !important;
    box-shadow: none !important;
    text-shadow: none !important;
  }
  a,
  a:visited {
    text-decoration: underline;
  }
  a[href]:after {
    content: " (" attr(href) ")";
  }
  abbr[title]:after {
    content: " (" attr(title) ")";
  }
  a[href^="#"]:after,
  a[href^="javascript:"]:after {
    content: "";
  }
  pre,
  blockquote {
    border: 1px solid #999;
    page-break-inside: avoid;
  }
  thead {
    display: table-header-group;
  }
  tr,
  img {
    page-break-inside: avoid;
  }
  img {
    max-width: 100% !important;
  }
  p,
  h2,
  h3 {
    orphans: 3;
    widows: 3;
  }
  h2,
  h3 {
    page-break-after: avoid;
  }
  .navbar {
    display: none;
  }
  .btn > .caret,
  .dropup > .btn > .caret {
    border-top-color: #000 !important;
  }
  .label {
    border: 1px solid #000;
  }
  .table {
    border-collapse: collapse !important;
  }
  .table td,
  .table th {
    background-color: #fff !important;
  }
  .table-bordered th,
  .table-bordered td {
    border: 1px solid #ddd !important;
  }
}
@font-face {
  font-family: 'Glyphicons Halflings';
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot');
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot?#iefix') format('embedded-opentype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff2') format('woff2'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff') format('woff'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.ttf') format('truetype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.svg#glyphicons_halflingsregular') format('svg');
}
.glyphicon {
  position: relative;
  top: 1px;
  display: inline-block;
  font-family: 'Glyphicons Halflings';
  font-style: normal;
  font-weight: normal;
  line-height: 1;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.glyphicon-asterisk:before {
  content: "\002a";
}
.glyphicon-plus:before {
  content: "\002b";
}
.glyphicon-euro:before,
.glyphicon-eur:before {
  content: "\20ac";
}
.glyphicon-minus:before {
  content: "\2212";
}
.glyphicon-cloud:before {
  content: "\2601";
}
.glyphicon-envelope:before {
  content: "\2709";
}
.glyphicon-pencil:before {
  content: "\270f";
}
.glyphicon-glass:before {
  content: "\e001";
}
.glyphicon-music:before {
  content: "\e002";
}
.glyphicon-search:before {
  content: "\e003";
}
.glyphicon-heart:before {
  content: "\e005";
}
.glyphicon-star:before {
  content: "\e006";
}
.glyphicon-star-empty:before {
  content: "\e007";
}
.glyphicon-user:before {
  content: "\e008";
}
.glyphicon-film:before {
  content: "\e009";
}
.glyphicon-th-large:before {
  content: "\e010";
}
.glyphicon-th:before {
  content: "\e011";
}
.glyphicon-th-list:before {
  content: "\e012";
}
.glyphicon-ok:before {
  content: "\e013";
}
.glyphicon-remove:before {
  content: "\e014";
}
.glyphicon-zoom-in:before {
  content: "\e015";
}
.glyphicon-zoom-out:before {
  content: "\e016";
}
.glyphicon-off:before {
  content: "\e017";
}
.glyphicon-signal:before {
  content: "\e018";
}
.glyphicon-cog:before {
  content: "\e019";
}
.glyphicon-trash:before {
  content: "\e020";
}
.glyphicon-home:before {
  content: "\e021";
}
.glyphicon-file:before {
  content: "\e022";
}
.glyphicon-time:before {
  content: "\e023";
}
.glyphicon-road:before {
  content: "\e024";
}
.glyphicon-download-alt:before {
  content: "\e025";
}
.glyphicon-download:before {
  content: "\e026";
}
.glyphicon-upload:before {
  content: "\e027";
}
.glyphicon-inbox:before {
  content: "\e028";
}
.glyphicon-play-circle:before {
  content: "\e029";
}
.glyphicon-repeat:before {
  content: "\e030";
}
.glyphicon-refresh:before {
  content: "\e031";
}
.glyphicon-list-alt:before {
  content: "\e032";
}
.glyphicon-lock:before {
  content: "\e033";
}
.glyphicon-flag:before {
  content: "\e034";
}
.glyphicon-headphones:before {
  content: "\e035";
}
.glyphicon-volume-off:before {
  content: "\e036";
}
.glyphicon-volume-down:before {
  content: "\e037";
}
.glyphicon-volume-up:before {
  content: "\e038";
}
.glyphicon-qrcode:before {
  content: "\e039";
}
.glyphicon-barcode:before {
  content: "\e040";
}
.glyphicon-tag:before {
  content: "\e041";
}
.glyphicon-tags:before {
  content: "\e042";
}
.glyphicon-book:before {
  content: "\e043";
}
.glyphicon-bookmark:before {
  content: "\e044";
}
.glyphicon-print:before {
  content: "\e045";
}
.glyphicon-camera:before {
  content: "\e046";
}
.glyphicon-font:before {
  content: "\e047";
}
.glyphicon-bold:before {
  content: "\e048";
}
.glyphicon-italic:before {
  content: "\e049";
}
.glyphicon-text-height:before {
  content: "\e050";
}
.glyphicon-text-width:before {
  content: "\e051";
}
.glyphicon-align-left:before {
  content: "\e052";
}
.glyphicon-align-center:before {
  content: "\e053";
}
.glyphicon-align-right:before {
  content: "\e054";
}
.glyphicon-align-justify:before {
  content: "\e055";
}
.glyphicon-list:before {
  content: "\e056";
}
.glyphicon-indent-left:before {
  content: "\e057";
}
.glyphicon-indent-right:before {
  content: "\e058";
}
.glyphicon-facetime-video:before {
  content: "\e059";
}
.glyphicon-picture:before {
  content: "\e060";
}
.glyphicon-map-marker:before {
  content: "\e062";
}
.glyphicon-adjust:before {
  content: "\e063";
}
.glyphicon-tint:before {
  content: "\e064";
}
.glyphicon-edit:before {
  content: "\e065";
}
.glyphicon-share:before {
  content: "\e066";
}
.glyphicon-check:before {
  content: "\e067";
}
.glyphicon-move:before {
  content: "\e068";
}
.glyphicon-step-backward:before {
  content: "\e069";
}
.glyphicon-fast-backward:before {
  content: "\e070";
}
.glyphicon-backward:before {
  content: "\e071";
}
.glyphicon-play:before {
  content: "\e072";
}
.glyphicon-pause:before {
  content: "\e073";
}
.glyphicon-stop:before {
  content: "\e074";
}
.glyphicon-forward:before {
  content: "\e075";
}
.glyphicon-fast-forward:before {
  content: "\e076";
}
.glyphicon-step-forward:before {
  content: "\e077";
}
.glyphicon-eject:before {
  content: "\e078";
}
.glyphicon-chevron-left:before {
  content: "\e079";
}
.glyphicon-chevron-right:before {
  content: "\e080";
}
.glyphicon-plus-sign:before {
  content: "\e081";
}
.glyphicon-minus-sign:before {
  content: "\e082";
}
.glyphicon-remove-sign:before {
  content: "\e083";
}
.glyphicon-ok-sign:before {
  content: "\e084";
}
.glyphicon-question-sign:before {
  content: "\e085";
}
.glyphicon-info-sign:before {
  content: "\e086";
}
.glyphicon-screenshot:before {
  content: "\e087";
}
.glyphicon-remove-circle:before {
  content: "\e088";
}
.glyphicon-ok-circle:before {
  content: "\e089";
}
.glyphicon-ban-circle:before {
  content: "\e090";
}
.glyphicon-arrow-left:before {
  content: "\e091";
}
.glyphicon-arrow-right:before {
  content: "\e092";
}
.glyphicon-arrow-up:before {
  content: "\e093";
}
.glyphicon-arrow-down:before {
  content: "\e094";
}
.glyphicon-share-alt:before {
  content: "\e095";
}
.glyphicon-resize-full:before {
  content: "\e096";
}
.glyphicon-resize-small:before {
  content: "\e097";
}
.glyphicon-exclamation-sign:before {
  content: "\e101";
}
.glyphicon-gift:before {
  content: "\e102";
}
.glyphicon-leaf:before {
  content: "\e103";
}
.glyphicon-fire:before {
  content: "\e104";
}
.glyphicon-eye-open:before {
  content: "\e105";
}
.glyphicon-eye-close:before {
  content: "\e106";
}
.glyphicon-warning-sign:before {
  content: "\e107";
}
.glyphicon-plane:before {
  content: "\e108";
}
.glyphicon-calendar:before {
  content: "\e109";
}
.glyphicon-random:before {
  content: "\e110";
}
.glyphicon-comment:before {
  content: "\e111";
}
.glyphicon-magnet:before {
  content: "\e112";
}
.glyphicon-chevron-up:before {
  content: "\e113";
}
.glyphicon-chevron-down:before {
  content: "\e114";
}
.glyphicon-retweet:before {
  content: "\e115";
}
.glyphicon-shopping-cart:before {
  content: "\e116";
}
.glyphicon-folder-close:before {
  content: "\e117";
}
.glyphicon-folder-open:before {
  content: "\e118";
}
.glyphicon-resize-vertical:before {
  content: "\e119";
}
.glyphicon-resize-horizontal:before {
  content: "\e120";
}
.glyphicon-hdd:before {
  content: "\e121";
}
.glyphicon-bullhorn:before {
  content: "\e122";
}
.glyphicon-bell:before {
  content: "\e123";
}
.glyphicon-certificate:before {
  content: "\e124";
}
.glyphicon-thumbs-up:before {
  content: "\e125";
}
.glyphicon-thumbs-down:before {
  content: "\e126";
}
.glyphicon-hand-right:before {
  content: "\e127";
}
.glyphicon-hand-left:before {
  content: "\e128";
}
.glyphicon-hand-up:before {
  content: "\e129";
}
.glyphicon-hand-down:before {
  content: "\e130";
}
.glyphicon-circle-arrow-right:before {
  content: "\e131";
}
.glyphicon-circle-arrow-left:before {
  content: "\e132";
}
.glyphicon-circle-arrow-up:before {
  content: "\e133";
}
.glyphicon-circle-arrow-down:before {
  content: "\e134";
}
.glyphicon-globe:before {
  content: "\e135";
}
.glyphicon-wrench:before {
  content: "\e136";
}
.glyphicon-tasks:before {
  content: "\e137";
}
.glyphicon-filter:before {
  content: "\e138";
}
.glyphicon-briefcase:before {
  content: "\e139";
}
.glyphicon-fullscreen:before {
  content: "\e140";
}
.glyphicon-dashboard:before {
  content: "\e141";
}
.glyphicon-paperclip:before {
  content: "\e142";
}
.glyphicon-heart-empty:before {
  content: "\e143";
}
.glyphicon-link:before {
  content: "\e144";
}
.glyphicon-phone:before {
  content: "\e145";
}
.glyphicon-pushpin:before {
  content: "\e146";
}
.glyphicon-usd:before {
  content: "\e148";
}
.glyphicon-gbp:before {
  content: "\e149";
}
.glyphicon-sort:before {
  content: "\e150";
}
.glyphicon-sort-by-alphabet:before {
  content: "\e151";
}
.glyphicon-sort-by-alphabet-alt:before {
  content: "\e152";
}
.glyphicon-sort-by-order:before {
  content: "\e153";
}
.glyphicon-sort-by-order-alt:before {
  content: "\e154";
}
.glyphicon-sort-by-attributes:before {
  content: "\e155";
}
.glyphicon-sort-by-attributes-alt:before {
  content: "\e156";
}
.glyphicon-unchecked:before {
  content: "\e157";
}
.glyphicon-expand:before {
  content: "\e158";
}
.glyphicon-collapse-down:before {
  content: "\e159";
}
.glyphicon-collapse-up:before {
  content: "\e160";
}
.glyphicon-log-in:before {
  content: "\e161";
}
.glyphicon-flash:before {
  content: "\e162";
}
.glyphicon-log-out:before {
  content: "\e163";
}
.glyphicon-new-window:before {
  content: "\e164";
}
.glyphicon-record:before {
  content: "\e165";
}
.glyphicon-save:before {
  content: "\e166";
}
.glyphicon-open:before {
  content: "\e167";
}
.glyphicon-saved:before {
  content: "\e168";
}
.glyphicon-import:before {
  content: "\e169";
}
.glyphicon-export:before {
  content: "\e170";
}
.glyphicon-send:before {
  content: "\e171";
}
.glyphicon-floppy-disk:before {
  content: "\e172";
}
.glyphicon-floppy-saved:before {
  content: "\e173";
}
.glyphicon-floppy-remove:before {
  content: "\e174";
}
.glyphicon-floppy-save:before {
  content: "\e175";
}
.glyphicon-floppy-open:before {
  content: "\e176";
}
.glyphicon-credit-card:before {
  content: "\e177";
}
.glyphicon-transfer:before {
  content: "\e178";
}
.glyphicon-cutlery:before {
  content: "\e179";
}
.glyphicon-header:before {
  content: "\e180";
}
.glyphicon-compressed:before {
  content: "\e181";
}
.glyphicon-earphone:before {
  content: "\e182";
}
.glyphicon-phone-alt:before {
  content: "\e183";
}
.glyphicon-tower:before {
  content: "\e184";
}
.glyphicon-stats:before {
  content: "\e185";
}
.glyphicon-sd-video:before {
  content: "\e186";
}
.glyphicon-hd-video:before {
  content: "\e187";
}
.glyphicon-subtitles:before {
  content: "\e188";
}
.glyphicon-sound-stereo:before {
  content: "\e189";
}
.glyphicon-sound-dolby:before {
  content: "\e190";
}
.glyphicon-sound-5-1:before {
  content: "\e191";
}
.glyphicon-sound-6-1:before {
  content: "\e192";
}
.glyphicon-sound-7-1:before {
  content: "\e193";
}
.glyphicon-copyright-mark:before {
  content: "\e194";
}
.glyphicon-registration-mark:before {
  content: "\e195";
}
.glyphicon-cloud-download:before {
  content: "\e197";
}
.glyphicon-cloud-upload:before {
  content: "\e198";
}
.glyphicon-tree-conifer:before {
  content: "\e199";
}
.glyphicon-tree-deciduous:before {
  content: "\e200";
}
.glyphicon-cd:before {
  content: "\e201";
}
.glyphicon-save-file:before {
  content: "\e202";
}
.glyphicon-open-file:before {
  content: "\e203";
}
.glyphicon-level-up:before {
  content: "\e204";
}
.glyphicon-copy:before {
  content: "\e205";
}
.glyphicon-paste:before {
  content: "\e206";
}
.glyphicon-alert:before {
  content: "\e209";
}
.glyphicon-equalizer:before {
  content: "\e210";
}
.glyphicon-king:before {
  content: "\e211";
}
.glyphicon-queen:before {
  content: "\e212";
}
.glyphicon-pawn:before {
  content: "\e213";
}
.glyphicon-bishop:before {
  content: "\e214";
}
.glyphicon-knight:before {
  content: "\e215";
}
.glyphicon-baby-formula:before {
  content: "\e216";
}
.glyphicon-tent:before {
  content: "\26fa";
}
.glyphicon-blackboard:before {
  content: "\e218";
}
.glyphicon-bed:before {
  content: "\e219";
}
.glyphicon-apple:before {
  content: "\f8ff";
}
.glyphicon-erase:before {
  content: "\e221";
}
.glyphicon-hourglass:before {
  content: "\231b";
}
.glyphicon-lamp:before {
  content: "\e223";
}
.glyphicon-duplicate:before {
  content: "\e224";
}
.glyphicon-piggy-bank:before {
  content: "\e225";
}
.glyphicon-scissors:before {
  content: "\e226";
}
.glyphicon-bitcoin:before {
  content: "\e227";
}
.glyphicon-btc:before {
  content: "\e227";
}
.glyphicon-xbt:before {
  content: "\e227";
}
.glyphicon-yen:before {
  content: "\00a5";
}
.glyphicon-jpy:before {
  content: "\00a5";
}
.glyphicon-ruble:before {
  content: "\20bd";
}
.glyphicon-rub:before {
  content: "\20bd";
}
.glyphicon-scale:before {
  content: "\e230";
}
.glyphicon-ice-lolly:before {
  content: "\e231";
}
.glyphicon-ice-lolly-tasted:before {
  content: "\e232";
}
.glyphicon-education:before {
  content: "\e233";
}
.glyphicon-option-horizontal:before {
  content: "\e234";
}
.glyphicon-option-vertical:before {
  content: "\e235";
}
.glyphicon-menu-hamburger:before {
  content: "\e236";
}
.glyphicon-modal-window:before {
  content: "\e237";
}
.glyphicon-oil:before {
  content: "\e238";
}
.glyphicon-grain:before {
  content: "\e239";
}
.glyphicon-sunglasses:before {
  content: "\e240";
}
.glyphicon-text-size:before {
  content: "\e241";
}
.glyphicon-text-color:before {
  content: "\e242";
}
.glyphicon-text-background:before {
  content: "\e243";
}
.glyphicon-object-align-top:before {
  content: "\e244";
}
.glyphicon-object-align-bottom:before {
  content: "\e245";
}
.glyphicon-object-align-horizontal:before {
  content: "\e246";
}
.glyphicon-object-align-left:before {
  content: "\e247";
}
.glyphicon-object-align-vertical:before {
  content: "\e248";
}
.glyphicon-object-align-right:before {
  content: "\e249";
}
.glyphicon-triangle-right:before {
  content: "\e250";
}
.glyphicon-triangle-left:before {
  content: "\e251";
}
.glyphicon-triangle-bottom:before {
  content: "\e252";
}
.glyphicon-triangle-top:before {
  content: "\e253";
}
.glyphicon-console:before {
  content: "\e254";
}
.glyphicon-superscript:before {
  content: "\e255";
}
.glyphicon-subscript:before {
  content: "\e256";
}
.glyphicon-menu-left:before {
  content: "\e257";
}
.glyphicon-menu-right:before {
  content: "\e258";
}
.glyphicon-menu-down:before {
  content: "\e259";
}
.glyphicon-menu-up:before {
  content: "\e260";
}
* {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
*:before,
*:after {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
html {
  font-size: 10px;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

input,
button,
select,
textarea {
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
}
a {
  color: #337ab7;
  text-decoration: none;
}
a:hover,
a:focus {
  color: #23527c;
  text-decoration: underline;
}
a:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
figure {
  margin: 0;
}
img {
  vertical-align: middle;
}
.img-responsive,
.thumbnail > img,
.thumbnail a > img,
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  display: block;
  max-width: 100%;
  height: auto;
}
.img-rounded {
  border-radius: 3px;
}
.img-thumbnail {
  padding: 4px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: all 0.2s ease-in-out;
  -o-transition: all 0.2s ease-in-out;
  transition: all 0.2s ease-in-out;
  display: inline-block;
  max-width: 100%;
  height: auto;
}
.img-circle {
  border-radius: 50%;
}
hr {
  margin-top: 18px;
  margin-bottom: 18px;
  border: 0;
  border-top: 1px solid #eeeeee;
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
[role="button"] {
  cursor: pointer;
}
h1,
h2,
h3,
h4,
h5,
h6,
.h1,
.h2,
.h3,
.h4,
.h5,
.h6 {
  font-family: inherit;
  font-weight: 500;
  line-height: 1.1;
  color: inherit;
}
h1 small,
h2 small,
h3 small,
h4 small,
h5 small,
h6 small,
.h1 small,
.h2 small,
.h3 small,
.h4 small,
.h5 small,
.h6 small,
h1 .small,
h2 .small,
h3 .small,
h4 .small,
h5 .small,
h6 .small,
.h1 .small,
.h2 .small,
.h3 .small,
.h4 .small,
.h5 .small,
.h6 .small {
  font-weight: normal;
  line-height: 1;
  color: #777777;
}
h1,
.h1,
h2,
.h2,
h3,
.h3 {
  margin-top: 18px;
  margin-bottom: 9px;
}
h1 small,
.h1 small,
h2 small,
.h2 small,
h3 small,
.h3 small,
h1 .small,
.h1 .small,
h2 .small,
.h2 .small,
h3 .small,
.h3 .small {
  font-size: 65%;
}
h4,
.h4,
h5,
.h5,
h6,
.h6 {
  margin-top: 9px;
  margin-bottom: 9px;
}
h4 small,
.h4 small,
h5 small,
.h5 small,
h6 small,
.h6 small,
h4 .small,
.h4 .small,
h5 .small,
.h5 .small,
h6 .small,
.h6 .small {
  font-size: 75%;
}
h1,
.h1 {
  font-size: 33px;
}
h2,
.h2 {
  font-size: 27px;
}
h3,
.h3 {
  font-size: 23px;
}
h4,
.h4 {
  font-size: 17px;
}
h5,
.h5 {
  font-size: 13px;
}
h6,
.h6 {
  font-size: 12px;
}
p {
  margin: 0 0 9px;
}
.lead {
  margin-bottom: 18px;
  font-size: 14px;
  font-weight: 300;
  line-height: 1.4;
}
@media (min-width: 768px) {
  .lead {
    font-size: 19.5px;
  }
}
small,
.small {
  font-size: 92%;
}
mark,
.mark {
  background-color: #fcf8e3;
  padding: .2em;
}
.text-left {
  text-align: left;
}
.text-right {
  text-align: right;
}
.text-center {
  text-align: center;
}
.text-justify {
  text-align: justify;
}
.text-nowrap {
  white-space: nowrap;
}
.text-lowercase {
  text-transform: lowercase;
}
.text-uppercase {
  text-transform: uppercase;
}
.text-capitalize {
  text-transform: capitalize;
}
.text-muted {
  color: #777777;
}
.text-primary {
  color: #337ab7;
}
a.text-primary:hover,
a.text-primary:focus {
  color: #286090;
}
.text-success {
  color: #3c763d;
}
a.text-success:hover,
a.text-success:focus {
  color: #2b542c;
}
.text-info {
  color: #31708f;
}
a.text-info:hover,
a.text-info:focus {
  color: #245269;
}
.text-warning {
  color: #8a6d3b;
}
a.text-warning:hover,
a.text-warning:focus {
  color: #66512c;
}
.text-danger {
  color: #a94442;
}
a.text-danger:hover,
a.text-danger:focus {
  color: #843534;
}
.bg-primary {
  color: #fff;
  background-color: #337ab7;
}
a.bg-primary:hover,
a.bg-primary:focus {
  background-color: #286090;
}
.bg-success {
  background-color: #dff0d8;
}
a.bg-success:hover,
a.bg-success:focus {
  background-color: #c1e2b3;
}
.bg-info {
  background-color: #d9edf7;
}
a.bg-info:hover,
a.bg-info:focus {
  background-color: #afd9ee;
}
.bg-warning {
  background-color: #fcf8e3;
}
a.bg-warning:hover,
a.bg-warning:focus {
  background-color: #f7ecb5;
}
.bg-danger {
  background-color: #f2dede;
}
a.bg-danger:hover,
a.bg-danger:focus {
  background-color: #e4b9b9;
}
.page-header {
  padding-bottom: 8px;
  margin: 36px 0 18px;
  border-bottom: 1px solid #eeeeee;
}
ul,
ol {
  margin-top: 0;
  margin-bottom: 9px;
}
ul ul,
ol ul,
ul ol,
ol ol {
  margin-bottom: 0;
}
.list-unstyled {
  padding-left: 0;
  list-style: none;
}
.list-inline {
  padding-left: 0;
  list-style: none;
  margin-left: -5px;
}
.list-inline > li {
  display: inline-block;
  padding-left: 5px;
  padding-right: 5px;
}
dl {
  margin-top: 0;
  margin-bottom: 18px;
}
dt,
dd {
  line-height: 1.42857143;
}
dt {
  font-weight: bold;
}
dd {
  margin-left: 0;
}
@media (min-width: 541px) {
  .dl-horizontal dt {
    float: left;
    width: 160px;
    clear: left;
    text-align: right;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .dl-horizontal dd {
    margin-left: 180px;
  }
}
abbr[title],
abbr[data-original-title] {
  cursor: help;
  border-bottom: 1px dotted #777777;
}
.initialism {
  font-size: 90%;
  text-transform: uppercase;
}
blockquote {
  padding: 9px 18px;
  margin: 0 0 18px;
  font-size: inherit;
  border-left: 5px solid #eeeeee;
}
blockquote p:last-child,
blockquote ul:last-child,
blockquote ol:last-child {
  margin-bottom: 0;
}
blockquote footer,
blockquote small,
blockquote .small {
  display: block;
  font-size: 80%;
  line-height: 1.42857143;
  color: #777777;
}
blockquote footer:before,
blockquote small:before,
blockquote .small:before {
  content: '\2014 \00A0';
}
.blockquote-reverse,
blockquote.pull-right {
  padding-right: 15px;
  padding-left: 0;
  border-right: 5px solid #eeeeee;
  border-left: 0;
  text-align: right;
}
.blockquote-reverse footer:before,
blockquote.pull-right footer:before,
.blockquote-reverse small:before,
blockquote.pull-right small:before,
.blockquote-reverse .small:before,
blockquote.pull-right .small:before {
  content: '';
}
.blockquote-reverse footer:after,
blockquote.pull-right footer:after,
.blockquote-reverse small:after,
blockquote.pull-right small:after,
.blockquote-reverse .small:after,
blockquote.pull-right .small:after {
  content: '\00A0 \2014';
}
address {
  margin-bottom: 18px;
  font-style: normal;
  line-height: 1.42857143;
}
code,
kbd,
pre,
samp {
  font-family: monospace;
}
code {
  padding: 2px 4px;
  font-size: 90%;
  color: #c7254e;
  background-color: #f9f2f4;
  border-radius: 2px;
}
kbd {
  padding: 2px 4px;
  font-size: 90%;
  color: #888;
  background-color: transparent;
  border-radius: 1px;
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.25);
}
kbd kbd {
  padding: 0;
  font-size: 100%;
  font-weight: bold;
  box-shadow: none;
}
pre {
  display: block;
  padding: 8.5px;
  margin: 0 0 9px;
  font-size: 12px;
  line-height: 1.42857143;
  word-break: break-all;
  word-wrap: break-word;
  color: #333333;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 2px;
}
pre code {
  padding: 0;
  font-size: inherit;
  color: inherit;
  white-space: pre-wrap;
  background-color: transparent;
  border-radius: 0;
}
.pre-scrollable {
  max-height: 340px;
  overflow-y: scroll;
}
.container {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
@media (min-width: 768px) {
  .container {
    width: 768px;
  }
}
@media (min-width: 992px) {
  .container {
    width: 940px;
  }
}
@media (min-width: 1200px) {
  .container {
    width: 1140px;
  }
}
.container-fluid {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
.row {
  margin-left: 0px;
  margin-right: 0px;
}
.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1, .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2, .col-xs-3, .col-sm-3, .col-md-3, .col-lg-3, .col-xs-4, .col-sm-4, .col-md-4, .col-lg-4, .col-xs-5, .col-sm-5, .col-md-5, .col-lg-5, .col-xs-6, .col-sm-6, .col-md-6, .col-lg-6, .col-xs-7, .col-sm-7, .col-md-7, .col-lg-7, .col-xs-8, .col-sm-8, .col-md-8, .col-lg-8, .col-xs-9, .col-sm-9, .col-md-9, .col-lg-9, .col-xs-10, .col-sm-10, .col-md-10, .col-lg-10, .col-xs-11, .col-sm-11, .col-md-11, .col-lg-11, .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12 {
  position: relative;
  min-height: 1px;
  padding-left: 0px;
  padding-right: 0px;
}
.col-xs-1, .col-xs-2, .col-xs-3, .col-xs-4, .col-xs-5, .col-xs-6, .col-xs-7, .col-xs-8, .col-xs-9, .col-xs-10, .col-xs-11, .col-xs-12 {
  float: left;
}
.col-xs-12 {
  width: 100%;
}
.col-xs-11 {
  width: 91.66666667%;
}
.col-xs-10 {
  width: 83.33333333%;
}
.col-xs-9 {
  width: 75%;
}
.col-xs-8 {
  width: 66.66666667%;
}
.col-xs-7 {
  width: 58.33333333%;
}
.col-xs-6 {
  width: 50%;
}
.col-xs-5 {
  width: 41.66666667%;
}
.col-xs-4 {
  width: 33.33333333%;
}
.col-xs-3 {
  width: 25%;
}
.col-xs-2 {
  width: 16.66666667%;
}
.col-xs-1 {
  width: 8.33333333%;
}
.col-xs-pull-12 {
  right: 100%;
}
.col-xs-pull-11 {
  right: 91.66666667%;
}
.col-xs-pull-10 {
  right: 83.33333333%;
}
.col-xs-pull-9 {
  right: 75%;
}
.col-xs-pull-8 {
  right: 66.66666667%;
}
.col-xs-pull-7 {
  right: 58.33333333%;
}
.col-xs-pull-6 {
  right: 50%;
}
.col-xs-pull-5 {
  right: 41.66666667%;
}
.col-xs-pull-4 {
  right: 33.33333333%;
}
.col-xs-pull-3 {
  right: 25%;
}
.col-xs-pull-2 {
  right: 16.66666667%;
}
.col-xs-pull-1 {
  right: 8.33333333%;
}
.col-xs-pull-0 {
  right: auto;
}
.col-xs-push-12 {
  left: 100%;
}
.col-xs-push-11 {
  left: 91.66666667%;
}
.col-xs-push-10 {
  left: 83.33333333%;
}
.col-xs-push-9 {
  left: 75%;
}
.col-xs-push-8 {
  left: 66.66666667%;
}
.col-xs-push-7 {
  left: 58.33333333%;
}
.col-xs-push-6 {
  left: 50%;
}
.col-xs-push-5 {
  left: 41.66666667%;
}
.col-xs-push-4 {
  left: 33.33333333%;
}
.col-xs-push-3 {
  left: 25%;
}
.col-xs-push-2 {
  left: 16.66666667%;
}
.col-xs-push-1 {
  left: 8.33333333%;
}
.col-xs-push-0 {
  left: auto;
}
.col-xs-offset-12 {
  margin-left: 100%;
}
.col-xs-offset-11 {
  margin-left: 91.66666667%;
}
.col-xs-offset-10 {
  margin-left: 83.33333333%;
}
.col-xs-offset-9 {
  margin-left: 75%;
}
.col-xs-offset-8 {
  margin-left: 66.66666667%;
}
.col-xs-offset-7 {
  margin-left: 58.33333333%;
}
.col-xs-offset-6 {
  margin-left: 50%;
}
.col-xs-offset-5 {
  margin-left: 41.66666667%;
}
.col-xs-offset-4 {
  margin-left: 33.33333333%;
}
.col-xs-offset-3 {
  margin-left: 25%;
}
.col-xs-offset-2 {
  margin-left: 16.66666667%;
}
.col-xs-offset-1 {
  margin-left: 8.33333333%;
}
.col-xs-offset-0 {
  margin-left: 0%;
}
@media (min-width: 768px) {
  .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
    float: left;
  }
  .col-sm-12 {
    width: 100%;
  }
  .col-sm-11 {
    width: 91.66666667%;
  }
  .col-sm-10 {
    width: 83.33333333%;
  }
  .col-sm-9 {
    width: 75%;
  }
  .col-sm-8 {
    width: 66.66666667%;
  }
  .col-sm-7 {
    width: 58.33333333%;
  }
  .col-sm-6 {
    width: 50%;
  }
  .col-sm-5 {
    width: 41.66666667%;
  }
  .col-sm-4 {
    width: 33.33333333%;
  }
  .col-sm-3 {
    width: 25%;
  }
  .col-sm-2 {
    width: 16.66666667%;
  }
  .col-sm-1 {
    width: 8.33333333%;
  }
  .col-sm-pull-12 {
    right: 100%;
  }
  .col-sm-pull-11 {
    right: 91.66666667%;
  }
  .col-sm-pull-10 {
    right: 83.33333333%;
  }
  .col-sm-pull-9 {
    right: 75%;
  }
  .col-sm-pull-8 {
    right: 66.66666667%;
  }
  .col-sm-pull-7 {
    right: 58.33333333%;
  }
  .col-sm-pull-6 {
    right: 50%;
  }
  .col-sm-pull-5 {
    right: 41.66666667%;
  }
  .col-sm-pull-4 {
    right: 33.33333333%;
  }
  .col-sm-pull-3 {
    right: 25%;
  }
  .col-sm-pull-2 {
    right: 16.66666667%;
  }
  .col-sm-pull-1 {
    right: 8.33333333%;
  }
  .col-sm-pull-0 {
    right: auto;
  }
  .col-sm-push-12 {
    left: 100%;
  }
  .col-sm-push-11 {
    left: 91.66666667%;
  }
  .col-sm-push-10 {
    left: 83.33333333%;
  }
  .col-sm-push-9 {
    left: 75%;
  }
  .col-sm-push-8 {
    left: 66.66666667%;
  }
  .col-sm-push-7 {
    left: 58.33333333%;
  }
  .col-sm-push-6 {
    left: 50%;
  }
  .col-sm-push-5 {
    left: 41.66666667%;
  }
  .col-sm-push-4 {
    left: 33.33333333%;
  }
  .col-sm-push-3 {
    left: 25%;
  }
  .col-sm-push-2 {
    left: 16.66666667%;
  }
  .col-sm-push-1 {
    left: 8.33333333%;
  }
  .col-sm-push-0 {
    left: auto;
  }
  .col-sm-offset-12 {
    margin-left: 100%;
  }
  .col-sm-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-sm-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-sm-offset-9 {
    margin-left: 75%;
  }
  .col-sm-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-sm-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-sm-offset-6 {
    margin-left: 50%;
  }
  .col-sm-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-sm-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-sm-offset-3 {
    margin-left: 25%;
  }
  .col-sm-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-sm-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-sm-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 992px) {
  .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6, .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
    float: left;
  }
  .col-md-12 {
    width: 100%;
  }
  .col-md-11 {
    width: 91.66666667%;
  }
  .col-md-10 {
    width: 83.33333333%;
  }
  .col-md-9 {
    width: 75%;
  }
  .col-md-8 {
    width: 66.66666667%;
  }
  .col-md-7 {
    width: 58.33333333%;
  }
  .col-md-6 {
    width: 50%;
  }
  .col-md-5 {
    width: 41.66666667%;
  }
  .col-md-4 {
    width: 33.33333333%;
  }
  .col-md-3 {
    width: 25%;
  }
  .col-md-2 {
    width: 16.66666667%;
  }
  .col-md-1 {
    width: 8.33333333%;
  }
  .col-md-pull-12 {
    right: 100%;
  }
  .col-md-pull-11 {
    right: 91.66666667%;
  }
  .col-md-pull-10 {
    right: 83.33333333%;
  }
  .col-md-pull-9 {
    right: 75%;
  }
  .col-md-pull-8 {
    right: 66.66666667%;
  }
  .col-md-pull-7 {
    right: 58.33333333%;
  }
  .col-md-pull-6 {
    right: 50%;
  }
  .col-md-pull-5 {
    right: 41.66666667%;
  }
  .col-md-pull-4 {
    right: 33.33333333%;
  }
  .col-md-pull-3 {
    right: 25%;
  }
  .col-md-pull-2 {
    right: 16.66666667%;
  }
  .col-md-pull-1 {
    right: 8.33333333%;
  }
  .col-md-pull-0 {
    right: auto;
  }
  .col-md-push-12 {
    left: 100%;
  }
  .col-md-push-11 {
    left: 91.66666667%;
  }
  .col-md-push-10 {
    left: 83.33333333%;
  }
  .col-md-push-9 {
    left: 75%;
  }
  .col-md-push-8 {
    left: 66.66666667%;
  }
  .col-md-push-7 {
    left: 58.33333333%;
  }
  .col-md-push-6 {
    left: 50%;
  }
  .col-md-push-5 {
    left: 41.66666667%;
  }
  .col-md-push-4 {
    left: 33.33333333%;
  }
  .col-md-push-3 {
    left: 25%;
  }
  .col-md-push-2 {
    left: 16.66666667%;
  }
  .col-md-push-1 {
    left: 8.33333333%;
  }
  .col-md-push-0 {
    left: auto;
  }
  .col-md-offset-12 {
    margin-left: 100%;
  }
  .col-md-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-md-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-md-offset-9 {
    margin-left: 75%;
  }
  .col-md-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-md-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-md-offset-6 {
    margin-left: 50%;
  }
  .col-md-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-md-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-md-offset-3 {
    margin-left: 25%;
  }
  .col-md-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-md-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-md-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 1200px) {
  .col-lg-1, .col-lg-2, .col-lg-3, .col-lg-4, .col-lg-5, .col-lg-6, .col-lg-7, .col-lg-8, .col-lg-9, .col-lg-10, .col-lg-11, .col-lg-12 {
    float: left;
  }
  .col-lg-12 {
    width: 100%;
  }
  .col-lg-11 {
    width: 91.66666667%;
  }
  .col-lg-10 {
    width: 83.33333333%;
  }
  .col-lg-9 {
    width: 75%;
  }
  .col-lg-8 {
    width: 66.66666667%;
  }
  .col-lg-7 {
    width: 58.33333333%;
  }
  .col-lg-6 {
    width: 50%;
  }
  .col-lg-5 {
    width: 41.66666667%;
  }
  .col-lg-4 {
    width: 33.33333333%;
  }
  .col-lg-3 {
    width: 25%;
  }
  .col-lg-2 {
    width: 16.66666667%;
  }
  .col-lg-1 {
    width: 8.33333333%;
  }
  .col-lg-pull-12 {
    right: 100%;
  }
  .col-lg-pull-11 {
    right: 91.66666667%;
  }
  .col-lg-pull-10 {
    right: 83.33333333%;
  }
  .col-lg-pull-9 {
    right: 75%;
  }
  .col-lg-pull-8 {
    right: 66.66666667%;
  }
  .col-lg-pull-7 {
    right: 58.33333333%;
  }
  .col-lg-pull-6 {
    right: 50%;
  }
  .col-lg-pull-5 {
    right: 41.66666667%;
  }
  .col-lg-pull-4 {
    right: 33.33333333%;
  }
  .col-lg-pull-3 {
    right: 25%;
  }
  .col-lg-pull-2 {
    right: 16.66666667%;
  }
  .col-lg-pull-1 {
    right: 8.33333333%;
  }
  .col-lg-pull-0 {
    right: auto;
  }
  .col-lg-push-12 {
    left: 100%;
  }
  .col-lg-push-11 {
    left: 91.66666667%;
  }
  .col-lg-push-10 {
    left: 83.33333333%;
  }
  .col-lg-push-9 {
    left: 75%;
  }
  .col-lg-push-8 {
    left: 66.66666667%;
  }
  .col-lg-push-7 {
    left: 58.33333333%;
  }
  .col-lg-push-6 {
    left: 50%;
  }
  .col-lg-push-5 {
    left: 41.66666667%;
  }
  .col-lg-push-4 {
    left: 33.33333333%;
  }
  .col-lg-push-3 {
    left: 25%;
  }
  .col-lg-push-2 {
    left: 16.66666667%;
  }
  .col-lg-push-1 {
    left: 8.33333333%;
  }
  .col-lg-push-0 {
    left: auto;
  }
  .col-lg-offset-12 {
    margin-left: 100%;
  }
  .col-lg-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-lg-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-lg-offset-9 {
    margin-left: 75%;
  }
  .col-lg-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-lg-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-lg-offset-6 {
    margin-left: 50%;
  }
  .col-lg-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-lg-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-lg-offset-3 {
    margin-left: 25%;
  }
  .col-lg-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-lg-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-lg-offset-0 {
    margin-left: 0%;
  }
}
table {
  background-color: transparent;
}
caption {
  padding-top: 8px;
  padding-bottom: 8px;
  color: #777777;
  text-align: left;
}
th {
  text-align: left;
}
.table {
  width: 100%;
  max-width: 100%;
  margin-bottom: 18px;
}
.table > thead > tr > th,
.table > tbody > tr > th,
.table > tfoot > tr > th,
.table > thead > tr > td,
.table > tbody > tr > td,
.table > tfoot > tr > td {
  padding: 8px;
  line-height: 1.42857143;
  vertical-align: top;
  border-top: 1px solid #ddd;
}
.table > thead > tr > th {
  vertical-align: bottom;
  border-bottom: 2px solid #ddd;
}
.table > caption + thead > tr:first-child > th,
.table > colgroup + thead > tr:first-child > th,
.table > thead:first-child > tr:first-child > th,
.table > caption + thead > tr:first-child > td,
.table > colgroup + thead > tr:first-child > td,
.table > thead:first-child > tr:first-child > td {
  border-top: 0;
}
.table > tbody + tbody {
  border-top: 2px solid #ddd;
}
.table .table {
  background-color: #fff;
}
.table-condensed > thead > tr > th,
.table-condensed > tbody > tr > th,
.table-condensed > tfoot > tr > th,
.table-condensed > thead > tr > td,
.table-condensed > tbody > tr > td,
.table-condensed > tfoot > tr > td {
  padding: 5px;
}
.table-bordered {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > tbody > tr > th,
.table-bordered > tfoot > tr > th,
.table-bordered > thead > tr > td,
.table-bordered > tbody > tr > td,
.table-bordered > tfoot > tr > td {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > thead > tr > td {
  border-bottom-width: 2px;
}
.table-striped > tbody > tr:nth-of-type(odd) {
  background-color: #f9f9f9;
}
.table-hover > tbody > tr:hover {
  background-color: #f5f5f5;
}
table col[class*="col-"] {
  position: static;
  float: none;
  display: table-column;
}
table td[class*="col-"],
table th[class*="col-"] {
  position: static;
  float: none;
  display: table-cell;
}
.table > thead > tr > td.active,
.table > tbody > tr > td.active,
.table > tfoot > tr > td.active,
.table > thead > tr > th.active,
.table > tbody > tr > th.active,
.table > tfoot > tr > th.active,
.table > thead > tr.active > td,
.table > tbody > tr.active > td,
.table > tfoot > tr.active > td,
.table > thead > tr.active > th,
.table > tbody > tr.active > th,
.table > tfoot > tr.active > th {
  background-color: #f5f5f5;
}
.table-hover > tbody > tr > td.active:hover,
.table-hover > tbody > tr > th.active:hover,
.table-hover > tbody > tr.active:hover > td,
.table-hover > tbody > tr:hover > .active,
.table-hover > tbody > tr.active:hover > th {
  background-color: #e8e8e8;
}
.table > thead > tr > td.success,
.table > tbody > tr > td.success,
.table > tfoot > tr > td.success,
.table > thead > tr > th.success,
.table > tbody > tr > th.success,
.table > tfoot > tr > th.success,
.table > thead > tr.success > td,
.table > tbody > tr.success > td,
.table > tfoot > tr.success > td,
.table > thead > tr.success > th,
.table > tbody > tr.success > th,
.table > tfoot > tr.success > th {
  background-color: #dff0d8;
}
.table-hover > tbody > tr > td.success:hover,
.table-hover > tbody > tr > th.success:hover,
.table-hover > tbody > tr.success:hover > td,
.table-hover > tbody > tr:hover > .success,
.table-hover > tbody > tr.success:hover > th {
  background-color: #d0e9c6;
}
.table > thead > tr > td.info,
.table > tbody > tr > td.info,
.table > tfoot > tr > td.info,
.table > thead > tr > th.info,
.table > tbody > tr > th.info,
.table > tfoot > tr > th.info,
.table > thead > tr.info > td,
.table > tbody > tr.info > td,
.table > tfoot > tr.info > td,
.table > thead > tr.info > th,
.table > tbody > tr.info > th,
.table > tfoot > tr.info > th {
  background-color: #d9edf7;
}
.table-hover > tbody > tr > td.info:hover,
.table-hover > tbody > tr > th.info:hover,
.table-hover > tbody > tr.info:hover > td,
.table-hover > tbody > tr:hover > .info,
.table-hover > tbody > tr.info:hover > th {
  background-color: #c4e3f3;
}
.table > thead > tr > td.warning,
.table > tbody > tr > td.warning,
.table > tfoot > tr > td.warning,
.table > thead > tr > th.warning,
.table > tbody > tr > th.warning,
.table > tfoot > tr > th.warning,
.table > thead > tr.warning > td,
.table > tbody > tr.warning > td,
.table > tfoot > tr.warning > td,
.table > thead > tr.warning > th,
.table > tbody > tr.warning > th,
.table > tfoot > tr.warning > th {
  background-color: #fcf8e3;
}
.table-hover > tbody > tr > td.warning:hover,
.table-hover > tbody > tr > th.warning:hover,
.table-hover > tbody > tr.warning:hover > td,
.table-hover > tbody > tr:hover > .warning,
.table-hover > tbody > tr.warning:hover > th {
  background-color: #faf2cc;
}
.table > thead > tr > td.danger,
.table > tbody > tr > td.danger,
.table > tfoot > tr > td.danger,
.table > thead > tr > th.danger,
.table > tbody > tr > th.danger,
.table > tfoot > tr > th.danger,
.table > thead > tr.danger > td,
.table > tbody > tr.danger > td,
.table > tfoot > tr.danger > td,
.table > thead > tr.danger > th,
.table > tbody > tr.danger > th,
.table > tfoot > tr.danger > th {
  background-color: #f2dede;
}
.table-hover > tbody > tr > td.danger:hover,
.table-hover > tbody > tr > th.danger:hover,
.table-hover > tbody > tr.danger:hover > td,
.table-hover > tbody > tr:hover > .danger,
.table-hover > tbody > tr.danger:hover > th {
  background-color: #ebcccc;
}
.table-responsive {
  overflow-x: auto;
  min-height: 0.01%;
}
@media screen and (max-width: 767px) {
  .table-responsive {
    width: 100%;
    margin-bottom: 13.5px;
    overflow-y: hidden;
    -ms-overflow-style: -ms-autohiding-scrollbar;
    border: 1px solid #ddd;
  }
  .table-responsive > .table {
    margin-bottom: 0;
  }
  .table-responsive > .table > thead > tr > th,
  .table-responsive > .table > tbody > tr > th,
  .table-responsive > .table > tfoot > tr > th,
  .table-responsive > .table > thead > tr > td,
  .table-responsive > .table > tbody > tr > td,
  .table-responsive > .table > tfoot > tr > td {
    white-space: nowrap;
  }
  .table-responsive > .table-bordered {
    border: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:first-child,
  .table-responsive > .table-bordered > tbody > tr > th:first-child,
  .table-responsive > .table-bordered > tfoot > tr > th:first-child,
  .table-responsive > .table-bordered > thead > tr > td:first-child,
  .table-responsive > .table-bordered > tbody > tr > td:first-child,
  .table-responsive > .table-bordered > tfoot > tr > td:first-child {
    border-left: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:last-child,
  .table-responsive > .table-bordered > tbody > tr > th:last-child,
  .table-responsive > .table-bordered > tfoot > tr > th:last-child,
  .table-responsive > .table-bordered > thead > tr > td:last-child,
  .table-responsive > .table-bordered > tbody > tr > td:last-child,
  .table-responsive > .table-bordered > tfoot > tr > td:last-child {
    border-right: 0;
  }
  .table-responsive > .table-bordered > tbody > tr:last-child > th,
  .table-responsive > .table-bordered > tfoot > tr:last-child > th,
  .table-responsive > .table-bordered > tbody > tr:last-child > td,
  .table-responsive > .table-bordered > tfoot > tr:last-child > td {
    border-bottom: 0;
  }
}
fieldset {
  padding: 0;
  margin: 0;
  border: 0;
  min-width: 0;
}
legend {
  display: block;
  width: 100%;
  padding: 0;
  margin-bottom: 18px;
  font-size: 19.5px;
  line-height: inherit;
  color: #333333;
  border: 0;
  border-bottom: 1px solid #e5e5e5;
}
label {
  display: inline-block;
  max-width: 100%;
  margin-bottom: 5px;
  font-weight: bold;
}
input[type="search"] {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
input[type="radio"],
input[type="checkbox"] {
  margin: 4px 0 0;
  margin-top: 1px \9;
  line-height: normal;
}
input[type="file"] {
  display: block;
}
input[type="range"] {
  display: block;
  width: 100%;
}
select[multiple],
select[size] {
  height: auto;
}
input[type="file"]:focus,
input[type="radio"]:focus,
input[type="checkbox"]:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
output {
  display: block;
  padding-top: 7px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
}
.form-control {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
}
.form-control:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.form-control::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.form-control:-ms-input-placeholder {
  color: #999;
}
.form-control::-webkit-input-placeholder {
  color: #999;
}
.form-control::-ms-expand {
  border: 0;
  background-color: transparent;
}
.form-control[disabled],
.form-control[readonly],
fieldset[disabled] .form-control {
  background-color: #eeeeee;
  opacity: 1;
}
.form-control[disabled],
fieldset[disabled] .form-control {
  cursor: not-allowed;
}
textarea.form-control {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: none;
}
@media screen and (-webkit-min-device-pixel-ratio: 0) {
  input[type="date"].form-control,
  input[type="time"].form-control,
  input[type="datetime-local"].form-control,
  input[type="month"].form-control {
    line-height: 32px;
  }
  input[type="date"].input-sm,
  input[type="time"].input-sm,
  input[type="datetime-local"].input-sm,
  input[type="month"].input-sm,
  .input-group-sm input[type="date"],
  .input-group-sm input[type="time"],
  .input-group-sm input[type="datetime-local"],
  .input-group-sm input[type="month"] {
    line-height: 30px;
  }
  input[type="date"].input-lg,
  input[type="time"].input-lg,
  input[type="datetime-local"].input-lg,
  input[type="month"].input-lg,
  .input-group-lg input[type="date"],
  .input-group-lg input[type="time"],
  .input-group-lg input[type="datetime-local"],
  .input-group-lg input[type="month"] {
    line-height: 45px;
  }
}
.form-group {
  margin-bottom: 15px;
}
.radio,
.checkbox {
  position: relative;
  display: block;
  margin-top: 10px;
  margin-bottom: 10px;
}
.radio label,
.checkbox label {
  min-height: 18px;
  padding-left: 20px;
  margin-bottom: 0;
  font-weight: normal;
  cursor: pointer;
}
.radio input[type="radio"],
.radio-inline input[type="radio"],
.checkbox input[type="checkbox"],
.checkbox-inline input[type="checkbox"] {
  position: absolute;
  margin-left: -20px;
  margin-top: 4px \9;
}
.radio + .radio,
.checkbox + .checkbox {
  margin-top: -5px;
}
.radio-inline,
.checkbox-inline {
  position: relative;
  display: inline-block;
  padding-left: 20px;
  margin-bottom: 0;
  vertical-align: middle;
  font-weight: normal;
  cursor: pointer;
}
.radio-inline + .radio-inline,
.checkbox-inline + .checkbox-inline {
  margin-top: 0;
  margin-left: 10px;
}
input[type="radio"][disabled],
input[type="checkbox"][disabled],
input[type="radio"].disabled,
input[type="checkbox"].disabled,
fieldset[disabled] input[type="radio"],
fieldset[disabled] input[type="checkbox"] {
  cursor: not-allowed;
}
.radio-inline.disabled,
.checkbox-inline.disabled,
fieldset[disabled] .radio-inline,
fieldset[disabled] .checkbox-inline {
  cursor: not-allowed;
}
.radio.disabled label,
.checkbox.disabled label,
fieldset[disabled] .radio label,
fieldset[disabled] .checkbox label {
  cursor: not-allowed;
}
.form-control-static {
  padding-top: 7px;
  padding-bottom: 7px;
  margin-bottom: 0;
  min-height: 31px;
}
.form-control-static.input-lg,
.form-control-static.input-sm {
  padding-left: 0;
  padding-right: 0;
}
.input-sm {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-sm {
  height: 30px;
  line-height: 30px;
}
textarea.input-sm,
select[multiple].input-sm {
  height: auto;
}
.form-group-sm .form-control {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.form-group-sm select.form-control {
  height: 30px;
  line-height: 30px;
}
.form-group-sm textarea.form-control,
.form-group-sm select[multiple].form-control {
  height: auto;
}
.form-group-sm .form-control-static {
  height: 30px;
  min-height: 30px;
  padding: 6px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.input-lg {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-lg {
  height: 45px;
  line-height: 45px;
}
textarea.input-lg,
select[multiple].input-lg {
  height: auto;
}
.form-group-lg .form-control {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.form-group-lg select.form-control {
  height: 45px;
  line-height: 45px;
}
.form-group-lg textarea.form-control,
.form-group-lg select[multiple].form-control {
  height: auto;
}
.form-group-lg .form-control-static {
  height: 45px;
  min-height: 35px;
  padding: 11px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.has-feedback {
  position: relative;
}
.has-feedback .form-control {
  padding-right: 40px;
}
.form-control-feedback {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 2;
  display: block;
  width: 32px;
  height: 32px;
  line-height: 32px;
  text-align: center;
  pointer-events: none;
}
.input-lg + .form-control-feedback,
.input-group-lg + .form-control-feedback,
.form-group-lg .form-control + .form-control-feedback {
  width: 45px;
  height: 45px;
  line-height: 45px;
}
.input-sm + .form-control-feedback,
.input-group-sm + .form-control-feedback,
.form-group-sm .form-control + .form-control-feedback {
  width: 30px;
  height: 30px;
  line-height: 30px;
}
.has-success .help-block,
.has-success .control-label,
.has-success .radio,
.has-success .checkbox,
.has-success .radio-inline,
.has-success .checkbox-inline,
.has-success.radio label,
.has-success.checkbox label,
.has-success.radio-inline label,
.has-success.checkbox-inline label {
  color: #3c763d;
}
.has-success .form-control {
  border-color: #3c763d;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-success .form-control:focus {
  border-color: #2b542c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
}
.has-success .input-group-addon {
  color: #3c763d;
  border-color: #3c763d;
  background-color: #dff0d8;
}
.has-success .form-control-feedback {
  color: #3c763d;
}
.has-warning .help-block,
.has-warning .control-label,
.has-warning .radio,
.has-warning .checkbox,
.has-warning .radio-inline,
.has-warning .checkbox-inline,
.has-warning.radio label,
.has-warning.checkbox label,
.has-warning.radio-inline label,
.has-warning.checkbox-inline label {
  color: #8a6d3b;
}
.has-warning .form-control {
  border-color: #8a6d3b;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-warning .form-control:focus {
  border-color: #66512c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
}
.has-warning .input-group-addon {
  color: #8a6d3b;
  border-color: #8a6d3b;
  background-color: #fcf8e3;
}
.has-warning .form-control-feedback {
  color: #8a6d3b;
}
.has-error .help-block,
.has-error .control-label,
.has-error .radio,
.has-error .checkbox,
.has-error .radio-inline,
.has-error .checkbox-inline,
.has-error.radio label,
.has-error.checkbox label,
.has-error.radio-inline label,
.has-error.checkbox-inline label {
  color: #a94442;
}
.has-error .form-control {
  border-color: #a94442;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-error .form-control:focus {
  border-color: #843534;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
}
.has-error .input-group-addon {
  color: #a94442;
  border-color: #a94442;
  background-color: #f2dede;
}
.has-error .form-control-feedback {
  color: #a94442;
}
.has-feedback label ~ .form-control-feedback {
  top: 23px;
}
.has-feedback label.sr-only ~ .form-control-feedback {
  top: 0;
}
.help-block {
  display: block;
  margin-top: 5px;
  margin-bottom: 10px;
  color: #404040;
}
@media (min-width: 768px) {
  .form-inline .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .form-inline .form-control-static {
    display: inline-block;
  }
  .form-inline .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .form-inline .input-group .input-group-addon,
  .form-inline .input-group .input-group-btn,
  .form-inline .input-group .form-control {
    width: auto;
  }
  .form-inline .input-group > .form-control {
    width: 100%;
  }
  .form-inline .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio,
  .form-inline .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio label,
  .form-inline .checkbox label {
    padding-left: 0;
  }
  .form-inline .radio input[type="radio"],
  .form-inline .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .form-inline .has-feedback .form-control-feedback {
    top: 0;
  }
}
.form-horizontal .radio,
.form-horizontal .checkbox,
.form-horizontal .radio-inline,
.form-horizontal .checkbox-inline {
  margin-top: 0;
  margin-bottom: 0;
  padding-top: 7px;
}
.form-horizontal .radio,
.form-horizontal .checkbox {
  min-height: 25px;
}
.form-horizontal .form-group {
  margin-left: 0px;
  margin-right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .control-label {
    text-align: right;
    margin-bottom: 0;
    padding-top: 7px;
  }
}
.form-horizontal .has-feedback .form-control-feedback {
  right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .form-group-lg .control-label {
    padding-top: 11px;
    font-size: 17px;
  }
}
@media (min-width: 768px) {
  .form-horizontal .form-group-sm .control-label {
    padding-top: 6px;
    font-size: 12px;
  }
}
.btn {
  display: inline-block;
  margin-bottom: 0;
  font-weight: normal;
  text-align: center;
  vertical-align: middle;
  touch-action: manipulation;
  cursor: pointer;
  background-image: none;
  border: 1px solid transparent;
  white-space: nowrap;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  border-radius: 2px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.btn:focus,
.btn:active:focus,
.btn.active:focus,
.btn.focus,
.btn:active.focus,
.btn.active.focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
.btn:hover,
.btn:focus,
.btn.focus {
  color: #333;
  text-decoration: none;
}
.btn:active,
.btn.active {
  outline: 0;
  background-image: none;
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn.disabled,
.btn[disabled],
fieldset[disabled] .btn {
  cursor: not-allowed;
  opacity: 0.65;
  filter: alpha(opacity=65);
  -webkit-box-shadow: none;
  box-shadow: none;
}
a.btn.disabled,
fieldset[disabled] a.btn {
  pointer-events: none;
}
.btn-default {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.btn-default:focus,
.btn-default.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.btn-default:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active:hover,
.btn-default.active:hover,
.open > .dropdown-toggle.btn-default:hover,
.btn-default:active:focus,
.btn-default.active:focus,
.open > .dropdown-toggle.btn-default:focus,
.btn-default:active.focus,
.btn-default.active.focus,
.open > .dropdown-toggle.btn-default.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  background-image: none;
}
.btn-default.disabled:hover,
.btn-default[disabled]:hover,
fieldset[disabled] .btn-default:hover,
.btn-default.disabled:focus,
.btn-default[disabled]:focus,
fieldset[disabled] .btn-default:focus,
.btn-default.disabled.focus,
.btn-default[disabled].focus,
fieldset[disabled] .btn-default.focus {
  background-color: #fff;
  border-color: #ccc;
}
.btn-default .badge {
  color: #fff;
  background-color: #333;
}
.btn-primary {
  color: #fff;
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary:focus,
.btn-primary.focus {
  color: #fff;
  background-color: #286090;
  border-color: #122b40;
}
.btn-primary:hover {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active:hover,
.btn-primary.active:hover,
.open > .dropdown-toggle.btn-primary:hover,
.btn-primary:active:focus,
.btn-primary.active:focus,
.open > .dropdown-toggle.btn-primary:focus,
.btn-primary:active.focus,
.btn-primary.active.focus,
.open > .dropdown-toggle.btn-primary.focus {
  color: #fff;
  background-color: #204d74;
  border-color: #122b40;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  background-image: none;
}
.btn-primary.disabled:hover,
.btn-primary[disabled]:hover,
fieldset[disabled] .btn-primary:hover,
.btn-primary.disabled:focus,
.btn-primary[disabled]:focus,
fieldset[disabled] .btn-primary:focus,
.btn-primary.disabled.focus,
.btn-primary[disabled].focus,
fieldset[disabled] .btn-primary.focus {
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary .badge {
  color: #337ab7;
  background-color: #fff;
}
.btn-success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success:focus,
.btn-success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.btn-success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active:hover,
.btn-success.active:hover,
.open > .dropdown-toggle.btn-success:hover,
.btn-success:active:focus,
.btn-success.active:focus,
.open > .dropdown-toggle.btn-success:focus,
.btn-success:active.focus,
.btn-success.active.focus,
.open > .dropdown-toggle.btn-success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  background-image: none;
}
.btn-success.disabled:hover,
.btn-success[disabled]:hover,
fieldset[disabled] .btn-success:hover,
.btn-success.disabled:focus,
.btn-success[disabled]:focus,
fieldset[disabled] .btn-success:focus,
.btn-success.disabled.focus,
.btn-success[disabled].focus,
fieldset[disabled] .btn-success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.btn-info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info:focus,
.btn-info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.btn-info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active:hover,
.btn-info.active:hover,
.open > .dropdown-toggle.btn-info:hover,
.btn-info:active:focus,
.btn-info.active:focus,
.open > .dropdown-toggle.btn-info:focus,
.btn-info:active.focus,
.btn-info.active.focus,
.open > .dropdown-toggle.btn-info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  background-image: none;
}
.btn-info.disabled:hover,
.btn-info[disabled]:hover,
fieldset[disabled] .btn-info:hover,
.btn-info.disabled:focus,
.btn-info[disabled]:focus,
fieldset[disabled] .btn-info:focus,
.btn-info.disabled.focus,
.btn-info[disabled].focus,
fieldset[disabled] .btn-info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.btn-warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning:focus,
.btn-warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.btn-warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active:hover,
.btn-warning.active:hover,
.open > .dropdown-toggle.btn-warning:hover,
.btn-warning:active:focus,
.btn-warning.active:focus,
.open > .dropdown-toggle.btn-warning:focus,
.btn-warning:active.focus,
.btn-warning.active.focus,
.open > .dropdown-toggle.btn-warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  background-image: none;
}
.btn-warning.disabled:hover,
.btn-warning[disabled]:hover,
fieldset[disabled] .btn-warning:hover,
.btn-warning.disabled:focus,
.btn-warning[disabled]:focus,
fieldset[disabled] .btn-warning:focus,
.btn-warning.disabled.focus,
.btn-warning[disabled].focus,
fieldset[disabled] .btn-warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.btn-danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger:focus,
.btn-danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.btn-danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active:hover,
.btn-danger.active:hover,
.open > .dropdown-toggle.btn-danger:hover,
.btn-danger:active:focus,
.btn-danger.active:focus,
.open > .dropdown-toggle.btn-danger:focus,
.btn-danger:active.focus,
.btn-danger.active.focus,
.open > .dropdown-toggle.btn-danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  background-image: none;
}
.btn-danger.disabled:hover,
.btn-danger[disabled]:hover,
fieldset[disabled] .btn-danger:hover,
.btn-danger.disabled:focus,
.btn-danger[disabled]:focus,
fieldset[disabled] .btn-danger:focus,
.btn-danger.disabled.focus,
.btn-danger[disabled].focus,
fieldset[disabled] .btn-danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger .badge {
  color: #d9534f;
  background-color: #fff;
}
.btn-link {
  color: #337ab7;
  font-weight: normal;
  border-radius: 0;
}
.btn-link,
.btn-link:active,
.btn-link.active,
.btn-link[disabled],
fieldset[disabled] .btn-link {
  background-color: transparent;
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn-link,
.btn-link:hover,
.btn-link:focus,
.btn-link:active {
  border-color: transparent;
}
.btn-link:hover,
.btn-link:focus {
  color: #23527c;
  text-decoration: underline;
  background-color: transparent;
}
.btn-link[disabled]:hover,
fieldset[disabled] .btn-link:hover,
.btn-link[disabled]:focus,
fieldset[disabled] .btn-link:focus {
  color: #777777;
  text-decoration: none;
}
.btn-lg,
.btn-group-lg > .btn {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.btn-sm,
.btn-group-sm > .btn {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-xs,
.btn-group-xs > .btn {
  padding: 1px 5px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-block {
  display: block;
  width: 100%;
}
.btn-block + .btn-block {
  margin-top: 5px;
}
input[type="submit"].btn-block,
input[type="reset"].btn-block,
input[type="button"].btn-block {
  width: 100%;
}
.fade {
  opacity: 0;
  -webkit-transition: opacity 0.15s linear;
  -o-transition: opacity 0.15s linear;
  transition: opacity 0.15s linear;
}
.fade.in {
  opacity: 1;
}
.collapse {
  display: none;
}
.collapse.in {
  display: block;
}
tr.collapse.in {
  display: table-row;
}
tbody.collapse.in {
  display: table-row-group;
}
.collapsing {
  position: relative;
  height: 0;
  overflow: hidden;
  -webkit-transition-property: height, visibility;
  transition-property: height, visibility;
  -webkit-transition-duration: 0.35s;
  transition-duration: 0.35s;
  -webkit-transition-timing-function: ease;
  transition-timing-function: ease;
}
.caret {
  display: inline-block;
  width: 0;
  height: 0;
  margin-left: 2px;
  vertical-align: middle;
  border-top: 4px dashed;
  border-top: 4px solid \9;
  border-right: 4px solid transparent;
  border-left: 4px solid transparent;
}
.dropup,
.dropdown {
  position: relative;
}
.dropdown-toggle:focus {
  outline: 0;
}
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  display: none;
  float: left;
  min-width: 160px;
  padding: 5px 0;
  margin: 2px 0 0;
  list-style: none;
  font-size: 13px;
  text-align: left;
  background-color: #fff;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.15);
  border-radius: 2px;
  -webkit-box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  background-clip: padding-box;
}
.dropdown-menu.pull-right {
  right: 0;
  left: auto;
}
.dropdown-menu .divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.dropdown-menu > li > a {
  display: block;
  padding: 3px 20px;
  clear: both;
  font-weight: normal;
  line-height: 1.42857143;
  color: #333333;
  white-space: nowrap;
}
.dropdown-menu > li > a:hover,
.dropdown-menu > li > a:focus {
  text-decoration: none;
  color: #262626;
  background-color: #f5f5f5;
}
.dropdown-menu > .active > a,
.dropdown-menu > .active > a:hover,
.dropdown-menu > .active > a:focus {
  color: #fff;
  text-decoration: none;
  outline: 0;
  background-color: #337ab7;
}
.dropdown-menu > .disabled > a,
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  color: #777777;
}
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  text-decoration: none;
  background-color: transparent;
  background-image: none;
  filter: progid:DXImageTransform.Microsoft.gradient(enabled = false);
  cursor: not-allowed;
}
.open > .dropdown-menu {
  display: block;
}
.open > a {
  outline: 0;
}
.dropdown-menu-right {
  left: auto;
  right: 0;
}
.dropdown-menu-left {
  left: 0;
  right: auto;
}
.dropdown-header {
  display: block;
  padding: 3px 20px;
  font-size: 12px;
  line-height: 1.42857143;
  color: #777777;
  white-space: nowrap;
}
.dropdown-backdrop {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  z-index: 990;
}
.pull-right > .dropdown-menu {
  right: 0;
  left: auto;
}
.dropup .caret,
.navbar-fixed-bottom .dropdown .caret {
  border-top: 0;
  border-bottom: 4px dashed;
  border-bottom: 4px solid \9;
  content: "";
}
.dropup .dropdown-menu,
.navbar-fixed-bottom .dropdown .dropdown-menu {
  top: auto;
  bottom: 100%;
  margin-bottom: 2px;
}
@media (min-width: 541px) {
  .navbar-right .dropdown-menu {
    left: auto;
    right: 0;
  }
  .navbar-right .dropdown-menu-left {
    left: 0;
    right: auto;
  }
}
.btn-group,
.btn-group-vertical {
  position: relative;
  display: inline-block;
  vertical-align: middle;
}
.btn-group > .btn,
.btn-group-vertical > .btn {
  position: relative;
  float: left;
}
.btn-group > .btn:hover,
.btn-group-vertical > .btn:hover,
.btn-group > .btn:focus,
.btn-group-vertical > .btn:focus,
.btn-group > .btn:active,
.btn-group-vertical > .btn:active,
.btn-group > .btn.active,
.btn-group-vertical > .btn.active {
  z-index: 2;
}
.btn-group .btn + .btn,
.btn-group .btn + .btn-group,
.btn-group .btn-group + .btn,
.btn-group .btn-group + .btn-group {
  margin-left: -1px;
}
.btn-toolbar {
  margin-left: -5px;
}
.btn-toolbar .btn,
.btn-toolbar .btn-group,
.btn-toolbar .input-group {
  float: left;
}
.btn-toolbar > .btn,
.btn-toolbar > .btn-group,
.btn-toolbar > .input-group {
  margin-left: 5px;
}
.btn-group > .btn:not(:first-child):not(:last-child):not(.dropdown-toggle) {
  border-radius: 0;
}
.btn-group > .btn:first-child {
  margin-left: 0;
}
.btn-group > .btn:first-child:not(:last-child):not(.dropdown-toggle) {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn:last-child:not(:first-child),
.btn-group > .dropdown-toggle:not(:first-child) {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group > .btn-group {
  float: left;
}
.btn-group > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group .dropdown-toggle:active,
.btn-group.open .dropdown-toggle {
  outline: 0;
}
.btn-group > .btn + .dropdown-toggle {
  padding-left: 8px;
  padding-right: 8px;
}
.btn-group > .btn-lg + .dropdown-toggle {
  padding-left: 12px;
  padding-right: 12px;
}
.btn-group.open .dropdown-toggle {
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn-group.open .dropdown-toggle.btn-link {
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn .caret {
  margin-left: 0;
}
.btn-lg .caret {
  border-width: 5px 5px 0;
  border-bottom-width: 0;
}
.dropup .btn-lg .caret {
  border-width: 0 5px 5px;
}
.btn-group-vertical > .btn,
.btn-group-vertical > .btn-group,
.btn-group-vertical > .btn-group > .btn {
  display: block;
  float: none;
  width: 100%;
  max-width: 100%;
}
.btn-group-vertical > .btn-group > .btn {
  float: none;
}
.btn-group-vertical > .btn + .btn,
.btn-group-vertical > .btn + .btn-group,
.btn-group-vertical > .btn-group + .btn,
.btn-group-vertical > .btn-group + .btn-group {
  margin-top: -1px;
  margin-left: 0;
}
.btn-group-vertical > .btn:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.btn-group-vertical > .btn:first-child:not(:last-child) {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn:last-child:not(:first-child) {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
.btn-group-vertical > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.btn-group-justified {
  display: table;
  width: 100%;
  table-layout: fixed;
  border-collapse: separate;
}
.btn-group-justified > .btn,
.btn-group-justified > .btn-group {
  float: none;
  display: table-cell;
  width: 1%;
}
.btn-group-justified > .btn-group .btn {
  width: 100%;
}
.btn-group-justified > .btn-group .dropdown-menu {
  left: auto;
}
[data-toggle="buttons"] > .btn input[type="radio"],
[data-toggle="buttons"] > .btn-group > .btn input[type="radio"],
[data-toggle="buttons"] > .btn input[type="checkbox"],
[data-toggle="buttons"] > .btn-group > .btn input[type="checkbox"] {
  position: absolute;
  clip: rect(0, 0, 0, 0);
  pointer-events: none;
}
.input-group {
  position: relative;
  display: table;
  border-collapse: separate;
}
.input-group[class*="col-"] {
  float: none;
  padding-left: 0;
  padding-right: 0;
}
.input-group .form-control {
  position: relative;
  z-index: 2;
  float: left;
  width: 100%;
  margin-bottom: 0;
}
.input-group .form-control:focus {
  z-index: 3;
}
.input-group-lg > .form-control,
.input-group-lg > .input-group-addon,
.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-group-lg > .form-control,
select.input-group-lg > .input-group-addon,
select.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  line-height: 45px;
}
textarea.input-group-lg > .form-control,
textarea.input-group-lg > .input-group-addon,
textarea.input-group-lg > .input-group-btn > .btn,
select[multiple].input-group-lg > .form-control,
select[multiple].input-group-lg > .input-group-addon,
select[multiple].input-group-lg > .input-group-btn > .btn {
  height: auto;
}
.input-group-sm > .form-control,
.input-group-sm > .input-group-addon,
.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-group-sm > .form-control,
select.input-group-sm > .input-group-addon,
select.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  line-height: 30px;
}
textarea.input-group-sm > .form-control,
textarea.input-group-sm > .input-group-addon,
textarea.input-group-sm > .input-group-btn > .btn,
select[multiple].input-group-sm > .form-control,
select[multiple].input-group-sm > .input-group-addon,
select[multiple].input-group-sm > .input-group-btn > .btn {
  height: auto;
}
.input-group-addon,
.input-group-btn,
.input-group .form-control {
  display: table-cell;
}
.input-group-addon:not(:first-child):not(:last-child),
.input-group-btn:not(:first-child):not(:last-child),
.input-group .form-control:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.input-group-addon,
.input-group-btn {
  width: 1%;
  white-space: nowrap;
  vertical-align: middle;
}
.input-group-addon {
  padding: 6px 12px;
  font-size: 13px;
  font-weight: normal;
  line-height: 1;
  color: #555555;
  text-align: center;
  background-color: #eeeeee;
  border: 1px solid #ccc;
  border-radius: 2px;
}
.input-group-addon.input-sm {
  padding: 5px 10px;
  font-size: 12px;
  border-radius: 1px;
}
.input-group-addon.input-lg {
  padding: 10px 16px;
  font-size: 17px;
  border-radius: 3px;
}
.input-group-addon input[type="radio"],
.input-group-addon input[type="checkbox"] {
  margin-top: 0;
}
.input-group .form-control:first-child,
.input-group-addon:first-child,
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group > .btn,
.input-group-btn:first-child > .dropdown-toggle,
.input-group-btn:last-child > .btn:not(:last-child):not(.dropdown-toggle),
.input-group-btn:last-child > .btn-group:not(:last-child) > .btn {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.input-group-addon:first-child {
  border-right: 0;
}
.input-group .form-control:last-child,
.input-group-addon:last-child,
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group > .btn,
.input-group-btn:last-child > .dropdown-toggle,
.input-group-btn:first-child > .btn:not(:first-child),
.input-group-btn:first-child > .btn-group:not(:first-child) > .btn {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.input-group-addon:last-child {
  border-left: 0;
}
.input-group-btn {
  position: relative;
  font-size: 0;
  white-space: nowrap;
}
.input-group-btn > .btn {
  position: relative;
}
.input-group-btn > .btn + .btn {
  margin-left: -1px;
}
.input-group-btn > .btn:hover,
.input-group-btn > .btn:focus,
.input-group-btn > .btn:active {
  z-index: 2;
}
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group {
  margin-right: -1px;
}
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group {
  z-index: 2;
  margin-left: -1px;
}
.nav {
  margin-bottom: 0;
  padding-left: 0;
  list-style: none;
}
.nav > li {
  position: relative;
  display: block;
}
.nav > li > a {
  position: relative;
  display: block;
  padding: 10px 15px;
}
.nav > li > a:hover,
.nav > li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.nav > li.disabled > a {
  color: #777777;
}
.nav > li.disabled > a:hover,
.nav > li.disabled > a:focus {
  color: #777777;
  text-decoration: none;
  background-color: transparent;
  cursor: not-allowed;
}
.nav .open > a,
.nav .open > a:hover,
.nav .open > a:focus {
  background-color: #eeeeee;
  border-color: #337ab7;
}
.nav .nav-divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.nav > li > a > img {
  max-width: none;
}
.nav-tabs {
  border-bottom: 1px solid #ddd;
}
.nav-tabs > li {
  float: left;
  margin-bottom: -1px;
}
.nav-tabs > li > a {
  margin-right: 2px;
  line-height: 1.42857143;
  border: 1px solid transparent;
  border-radius: 2px 2px 0 0;
}
.nav-tabs > li > a:hover {
  border-color: #eeeeee #eeeeee #ddd;
}
.nav-tabs > li.active > a,
.nav-tabs > li.active > a:hover,
.nav-tabs > li.active > a:focus {
  color: #555555;
  background-color: #fff;
  border: 1px solid #ddd;
  border-bottom-color: transparent;
  cursor: default;
}
.nav-tabs.nav-justified {
  width: 100%;
  border-bottom: 0;
}
.nav-tabs.nav-justified > li {
  float: none;
}
.nav-tabs.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-tabs.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-tabs.nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs.nav-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs.nav-justified > .active > a,
.nav-tabs.nav-justified > .active > a:hover,
.nav-tabs.nav-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs.nav-justified > .active > a,
  .nav-tabs.nav-justified > .active > a:hover,
  .nav-tabs.nav-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.nav-pills > li {
  float: left;
}
.nav-pills > li > a {
  border-radius: 2px;
}
.nav-pills > li + li {
  margin-left: 2px;
}
.nav-pills > li.active > a,
.nav-pills > li.active > a:hover,
.nav-pills > li.active > a:focus {
  color: #fff;
  background-color: #337ab7;
}
.nav-stacked > li {
  float: none;
}
.nav-stacked > li + li {
  margin-top: 2px;
  margin-left: 0;
}
.nav-justified {
  width: 100%;
}
.nav-justified > li {
  float: none;
}
.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs-justified {
  border-bottom: 0;
}
.nav-tabs-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs-justified > .active > a,
.nav-tabs-justified > .active > a:hover,
.nav-tabs-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs-justified > .active > a,
  .nav-tabs-justified > .active > a:hover,
  .nav-tabs-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.tab-content > .tab-pane {
  display: none;
}
.tab-content > .active {
  display: block;
}
.nav-tabs .dropdown-menu {
  margin-top: -1px;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar {
  position: relative;
  min-height: 30px;
  margin-bottom: 18px;
  border: 1px solid transparent;
}
@media (min-width: 541px) {
  .navbar {
    border-radius: 2px;
  }
}
@media (min-width: 541px) {
  .navbar-header {
    float: left;
  }
}
.navbar-collapse {
  overflow-x: visible;
  padding-right: 0px;
  padding-left: 0px;
  border-top: 1px solid transparent;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1);
  -webkit-overflow-scrolling: touch;
}
.navbar-collapse.in {
  overflow-y: auto;
}
@media (min-width: 541px) {
  .navbar-collapse {
    width: auto;
    border-top: 0;
    box-shadow: none;
  }
  .navbar-collapse.collapse {
    display: block !important;
    height: auto !important;
    padding-bottom: 0;
    overflow: visible !important;
  }
  .navbar-collapse.in {
    overflow-y: visible;
  }
  .navbar-fixed-top .navbar-collapse,
  .navbar-static-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    padding-left: 0;
    padding-right: 0;
  }
}
.navbar-fixed-top .navbar-collapse,
.navbar-fixed-bottom .navbar-collapse {
  max-height: 340px;
}
@media (max-device-width: 540px) and (orientation: landscape) {
  .navbar-fixed-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    max-height: 200px;
  }
}
.container > .navbar-header,
.container-fluid > .navbar-header,
.container > .navbar-collapse,
.container-fluid > .navbar-collapse {
  margin-right: 0px;
  margin-left: 0px;
}
@media (min-width: 541px) {
  .container > .navbar-header,
  .container-fluid > .navbar-header,
  .container > .navbar-collapse,
  .container-fluid > .navbar-collapse {
    margin-right: 0;
    margin-left: 0;
  }
}
.navbar-static-top {
  z-index: 1000;
  border-width: 0 0 1px;
}
@media (min-width: 541px) {
  .navbar-static-top {
    border-radius: 0;
  }
}
.navbar-fixed-top,
.navbar-fixed-bottom {
  position: fixed;
  right: 0;
  left: 0;
  z-index: 1030;
}
@media (min-width: 541px) {
  .navbar-fixed-top,
  .navbar-fixed-bottom {
    border-radius: 0;
  }
}
.navbar-fixed-top {
  top: 0;
  border-width: 0 0 1px;
}
.navbar-fixed-bottom {
  bottom: 0;
  margin-bottom: 0;
  border-width: 1px 0 0;
}
.navbar-brand {
  float: left;
  padding: 6px 0px;
  font-size: 17px;
  line-height: 18px;
  height: 30px;
}
.navbar-brand:hover,
.navbar-brand:focus {
  text-decoration: none;
}
.navbar-brand > img {
  display: block;
}
@media (min-width: 541px) {
  .navbar > .container .navbar-brand,
  .navbar > .container-fluid .navbar-brand {
    margin-left: 0px;
  }
}
.navbar-toggle {
  position: relative;
  float: right;
  margin-right: 0px;
  padding: 9px 10px;
  margin-top: -2px;
  margin-bottom: -2px;
  background-color: transparent;
  background-image: none;
  border: 1px solid transparent;
  border-radius: 2px;
}
.navbar-toggle:focus {
  outline: 0;
}
.navbar-toggle .icon-bar {
  display: block;
  width: 22px;
  height: 2px;
  border-radius: 1px;
}
.navbar-toggle .icon-bar + .icon-bar {
  margin-top: 4px;
}
@media (min-width: 541px) {
  .navbar-toggle {
    display: none;
  }
}
.navbar-nav {
  margin: 3px 0px;
}
.navbar-nav > li > a {
  padding-top: 10px;
  padding-bottom: 10px;
  line-height: 18px;
}
@media (max-width: 540px) {
  .navbar-nav .open .dropdown-menu {
    position: static;
    float: none;
    width: auto;
    margin-top: 0;
    background-color: transparent;
    border: 0;
    box-shadow: none;
  }
  .navbar-nav .open .dropdown-menu > li > a,
  .navbar-nav .open .dropdown-menu .dropdown-header {
    padding: 5px 15px 5px 25px;
  }
  .navbar-nav .open .dropdown-menu > li > a {
    line-height: 18px;
  }
  .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-nav .open .dropdown-menu > li > a:focus {
    background-image: none;
  }
}
@media (min-width: 541px) {
  .navbar-nav {
    float: left;
    margin: 0;
  }
  .navbar-nav > li {
    float: left;
  }
  .navbar-nav > li > a {
    padding-top: 6px;
    padding-bottom: 6px;
  }
}
.navbar-form {
  margin-left: 0px;
  margin-right: 0px;
  padding: 10px 0px;
  border-top: 1px solid transparent;
  border-bottom: 1px solid transparent;
  -webkit-box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  margin-top: -1px;
  margin-bottom: -1px;
}
@media (min-width: 768px) {
  .navbar-form .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .navbar-form .form-control-static {
    display: inline-block;
  }
  .navbar-form .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .navbar-form .input-group .input-group-addon,
  .navbar-form .input-group .input-group-btn,
  .navbar-form .input-group .form-control {
    width: auto;
  }
  .navbar-form .input-group > .form-control {
    width: 100%;
  }
  .navbar-form .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio,
  .navbar-form .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio label,
  .navbar-form .checkbox label {
    padding-left: 0;
  }
  .navbar-form .radio input[type="radio"],
  .navbar-form .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .navbar-form .has-feedback .form-control-feedback {
    top: 0;
  }
}
@media (max-width: 540px) {
  .navbar-form .form-group {
    margin-bottom: 5px;
  }
  .navbar-form .form-group:last-child {
    margin-bottom: 0;
  }
}
@media (min-width: 541px) {
  .navbar-form {
    width: auto;
    border: 0;
    margin-left: 0;
    margin-right: 0;
    padding-top: 0;
    padding-bottom: 0;
    -webkit-box-shadow: none;
    box-shadow: none;
  }
}
.navbar-nav > li > .dropdown-menu {
  margin-top: 0;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar-fixed-bottom .navbar-nav > li > .dropdown-menu {
  margin-bottom: 0;
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.navbar-btn {
  margin-top: -1px;
  margin-bottom: -1px;
}
.navbar-btn.btn-sm {
  margin-top: 0px;
  margin-bottom: 0px;
}
.navbar-btn.btn-xs {
  margin-top: 4px;
  margin-bottom: 4px;
}
.navbar-text {
  margin-top: 6px;
  margin-bottom: 6px;
}
@media (min-width: 541px) {
  .navbar-text {
    float: left;
    margin-left: 0px;
    margin-right: 0px;
  }
}
@media (min-width: 541px) {
  .navbar-left {
    float: left !important;
    float: left;
  }
  .navbar-right {
    float: right !important;
    float: right;
    margin-right: 0px;
  }
  .navbar-right ~ .navbar-right {
    margin-right: 0;
  }
}
.navbar-default {
  background-color: #f8f8f8;
  border-color: #e7e7e7;
}
.navbar-default .navbar-brand {
  color: #777;
}
.navbar-default .navbar-brand:hover,
.navbar-default .navbar-brand:focus {
  color: #5e5e5e;
  background-color: transparent;
}
.navbar-default .navbar-text {
  color: #777;
}
.navbar-default .navbar-nav > li > a {
  color: #777;
}
.navbar-default .navbar-nav > li > a:hover,
.navbar-default .navbar-nav > li > a:focus {
  color: #333;
  background-color: transparent;
}
.navbar-default .navbar-nav > .active > a,
.navbar-default .navbar-nav > .active > a:hover,
.navbar-default .navbar-nav > .active > a:focus {
  color: #555;
  background-color: #e7e7e7;
}
.navbar-default .navbar-nav > .disabled > a,
.navbar-default .navbar-nav > .disabled > a:hover,
.navbar-default .navbar-nav > .disabled > a:focus {
  color: #ccc;
  background-color: transparent;
}
.navbar-default .navbar-toggle {
  border-color: #ddd;
}
.navbar-default .navbar-toggle:hover,
.navbar-default .navbar-toggle:focus {
  background-color: #ddd;
}
.navbar-default .navbar-toggle .icon-bar {
  background-color: #888;
}
.navbar-default .navbar-collapse,
.navbar-default .navbar-form {
  border-color: #e7e7e7;
}
.navbar-default .navbar-nav > .open > a,
.navbar-default .navbar-nav > .open > a:hover,
.navbar-default .navbar-nav > .open > a:focus {
  background-color: #e7e7e7;
  color: #555;
}
@media (max-width: 540px) {
  .navbar-default .navbar-nav .open .dropdown-menu > li > a {
    color: #777;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #333;
    background-color: transparent;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #555;
    background-color: #e7e7e7;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #ccc;
    background-color: transparent;
  }
}
.navbar-default .navbar-link {
  color: #777;
}
.navbar-default .navbar-link:hover {
  color: #333;
}
.navbar-default .btn-link {
  color: #777;
}
.navbar-default .btn-link:hover,
.navbar-default .btn-link:focus {
  color: #333;
}
.navbar-default .btn-link[disabled]:hover,
fieldset[disabled] .navbar-default .btn-link:hover,
.navbar-default .btn-link[disabled]:focus,
fieldset[disabled] .navbar-default .btn-link:focus {
  color: #ccc;
}
.navbar-inverse {
  background-color: #222;
  border-color: #080808;
}
.navbar-inverse .navbar-brand {
  color: #9d9d9d;
}
.navbar-inverse .navbar-brand:hover,
.navbar-inverse .navbar-brand:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-text {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a:hover,
.navbar-inverse .navbar-nav > li > a:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-nav > .active > a,
.navbar-inverse .navbar-nav > .active > a:hover,
.navbar-inverse .navbar-nav > .active > a:focus {
  color: #fff;
  background-color: #080808;
}
.navbar-inverse .navbar-nav > .disabled > a,
.navbar-inverse .navbar-nav > .disabled > a:hover,
.navbar-inverse .navbar-nav > .disabled > a:focus {
  color: #444;
  background-color: transparent;
}
.navbar-inverse .navbar-toggle {
  border-color: #333;
}
.navbar-inverse .navbar-toggle:hover,
.navbar-inverse .navbar-toggle:focus {
  background-color: #333;
}
.navbar-inverse .navbar-toggle .icon-bar {
  background-color: #fff;
}
.navbar-inverse .navbar-collapse,
.navbar-inverse .navbar-form {
  border-color: #101010;
}
.navbar-inverse .navbar-nav > .open > a,
.navbar-inverse .navbar-nav > .open > a:hover,
.navbar-inverse .navbar-nav > .open > a:focus {
  background-color: #080808;
  color: #fff;
}
@media (max-width: 540px) {
  .navbar-inverse .navbar-nav .open .dropdown-menu > .dropdown-header {
    border-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu .divider {
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a {
    color: #9d9d9d;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #fff;
    background-color: transparent;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #fff;
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #444;
    background-color: transparent;
  }
}
.navbar-inverse .navbar-link {
  color: #9d9d9d;
}
.navbar-inverse .navbar-link:hover {
  color: #fff;
}
.navbar-inverse .btn-link {
  color: #9d9d9d;
}
.navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link:focus {
  color: #fff;
}
.navbar-inverse .btn-link[disabled]:hover,
fieldset[disabled] .navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link[disabled]:focus,
fieldset[disabled] .navbar-inverse .btn-link:focus {
  color: #444;
}
.breadcrumb {
  padding: 8px 15px;
  margin-bottom: 18px;
  list-style: none;
  background-color: #f5f5f5;
  border-radius: 2px;
}
.breadcrumb > li {
  display: inline-block;
}
.breadcrumb > li + li:before {
  content: "/\00a0";
  padding: 0 5px;
  color: #5e5e5e;
}
.breadcrumb > .active {
  color: #777777;
}
.pagination {
  display: inline-block;
  padding-left: 0;
  margin: 18px 0;
  border-radius: 2px;
}
.pagination > li {
  display: inline;
}
.pagination > li > a,
.pagination > li > span {
  position: relative;
  float: left;
  padding: 6px 12px;
  line-height: 1.42857143;
  text-decoration: none;
  color: #337ab7;
  background-color: #fff;
  border: 1px solid #ddd;
  margin-left: -1px;
}
.pagination > li:first-child > a,
.pagination > li:first-child > span {
  margin-left: 0;
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.pagination > li:last-child > a,
.pagination > li:last-child > span {
  border-bottom-right-radius: 2px;
  border-top-right-radius: 2px;
}
.pagination > li > a:hover,
.pagination > li > span:hover,
.pagination > li > a:focus,
.pagination > li > span:focus {
  z-index: 2;
  color: #23527c;
  background-color: #eeeeee;
  border-color: #ddd;
}
.pagination > .active > a,
.pagination > .active > span,
.pagination > .active > a:hover,
.pagination > .active > span:hover,
.pagination > .active > a:focus,
.pagination > .active > span:focus {
  z-index: 3;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
  cursor: default;
}
.pagination > .disabled > span,
.pagination > .disabled > span:hover,
.pagination > .disabled > span:focus,
.pagination > .disabled > a,
.pagination > .disabled > a:hover,
.pagination > .disabled > a:focus {
  color: #777777;
  background-color: #fff;
  border-color: #ddd;
  cursor: not-allowed;
}
.pagination-lg > li > a,
.pagination-lg > li > span {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.pagination-lg > li:first-child > a,
.pagination-lg > li:first-child > span {
  border-bottom-left-radius: 3px;
  border-top-left-radius: 3px;
}
.pagination-lg > li:last-child > a,
.pagination-lg > li:last-child > span {
  border-bottom-right-radius: 3px;
  border-top-right-radius: 3px;
}
.pagination-sm > li > a,
.pagination-sm > li > span {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.pagination-sm > li:first-child > a,
.pagination-sm > li:first-child > span {
  border-bottom-left-radius: 1px;
  border-top-left-radius: 1px;
}
.pagination-sm > li:last-child > a,
.pagination-sm > li:last-child > span {
  border-bottom-right-radius: 1px;
  border-top-right-radius: 1px;
}
.pager {
  padding-left: 0;
  margin: 18px 0;
  list-style: none;
  text-align: center;
}
.pager li {
  display: inline;
}
.pager li > a,
.pager li > span {
  display: inline-block;
  padding: 5px 14px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 15px;
}
.pager li > a:hover,
.pager li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.pager .next > a,
.pager .next > span {
  float: right;
}
.pager .previous > a,
.pager .previous > span {
  float: left;
}
.pager .disabled > a,
.pager .disabled > a:hover,
.pager .disabled > a:focus,
.pager .disabled > span {
  color: #777777;
  background-color: #fff;
  cursor: not-allowed;
}
.label {
  display: inline;
  padding: .2em .6em .3em;
  font-size: 75%;
  font-weight: bold;
  line-height: 1;
  color: #fff;
  text-align: center;
  white-space: nowrap;
  vertical-align: baseline;
  border-radius: .25em;
}
a.label:hover,
a.label:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.label:empty {
  display: none;
}
.btn .label {
  position: relative;
  top: -1px;
}
.label-default {
  background-color: #777777;
}
.label-default[href]:hover,
.label-default[href]:focus {
  background-color: #5e5e5e;
}
.label-primary {
  background-color: #337ab7;
}
.label-primary[href]:hover,
.label-primary[href]:focus {
  background-color: #286090;
}
.label-success {
  background-color: #5cb85c;
}
.label-success[href]:hover,
.label-success[href]:focus {
  background-color: #449d44;
}
.label-info {
  background-color: #5bc0de;
}
.label-info[href]:hover,
.label-info[href]:focus {
  background-color: #31b0d5;
}
.label-warning {
  background-color: #f0ad4e;
}
.label-warning[href]:hover,
.label-warning[href]:focus {
  background-color: #ec971f;
}
.label-danger {
  background-color: #d9534f;
}
.label-danger[href]:hover,
.label-danger[href]:focus {
  background-color: #c9302c;
}
.badge {
  display: inline-block;
  min-width: 10px;
  padding: 3px 7px;
  font-size: 12px;
  font-weight: bold;
  color: #fff;
  line-height: 1;
  vertical-align: middle;
  white-space: nowrap;
  text-align: center;
  background-color: #777777;
  border-radius: 10px;
}
.badge:empty {
  display: none;
}
.btn .badge {
  position: relative;
  top: -1px;
}
.btn-xs .badge,
.btn-group-xs > .btn .badge {
  top: 0;
  padding: 1px 5px;
}
a.badge:hover,
a.badge:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.list-group-item.active > .badge,
.nav-pills > .active > a > .badge {
  color: #337ab7;
  background-color: #fff;
}
.list-group-item > .badge {
  float: right;
}
.list-group-item > .badge + .badge {
  margin-right: 5px;
}
.nav-pills > li > a > .badge {
  margin-left: 3px;
}
.jumbotron {
  padding-top: 30px;
  padding-bottom: 30px;
  margin-bottom: 30px;
  color: inherit;
  background-color: #eeeeee;
}
.jumbotron h1,
.jumbotron .h1 {
  color: inherit;
}
.jumbotron p {
  margin-bottom: 15px;
  font-size: 20px;
  font-weight: 200;
}
.jumbotron > hr {
  border-top-color: #d5d5d5;
}
.container .jumbotron,
.container-fluid .jumbotron {
  border-radius: 3px;
  padding-left: 0px;
  padding-right: 0px;
}
.jumbotron .container {
  max-width: 100%;
}
@media screen and (min-width: 768px) {
  .jumbotron {
    padding-top: 48px;
    padding-bottom: 48px;
  }
  .container .jumbotron,
  .container-fluid .jumbotron {
    padding-left: 60px;
    padding-right: 60px;
  }
  .jumbotron h1,
  .jumbotron .h1 {
    font-size: 59px;
  }
}
.thumbnail {
  display: block;
  padding: 4px;
  margin-bottom: 18px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: border 0.2s ease-in-out;
  -o-transition: border 0.2s ease-in-out;
  transition: border 0.2s ease-in-out;
}
.thumbnail > img,
.thumbnail a > img {
  margin-left: auto;
  margin-right: auto;
}
a.thumbnail:hover,
a.thumbnail:focus,
a.thumbnail.active {
  border-color: #337ab7;
}
.thumbnail .caption {
  padding: 9px;
  color: #000;
}
.alert {
  padding: 15px;
  margin-bottom: 18px;
  border: 1px solid transparent;
  border-radius: 2px;
}
.alert h4 {
  margin-top: 0;
  color: inherit;
}
.alert .alert-link {
  font-weight: bold;
}
.alert > p,
.alert > ul {
  margin-bottom: 0;
}
.alert > p + p {
  margin-top: 5px;
}
.alert-dismissable,
.alert-dismissible {
  padding-right: 35px;
}
.alert-dismissable .close,
.alert-dismissible .close {
  position: relative;
  top: -2px;
  right: -21px;
  color: inherit;
}
.alert-success {
  background-color: #dff0d8;
  border-color: #d6e9c6;
  color: #3c763d;
}
.alert-success hr {
  border-top-color: #c9e2b3;
}
.alert-success .alert-link {
  color: #2b542c;
}
.alert-info {
  background-color: #d9edf7;
  border-color: #bce8f1;
  color: #31708f;
}
.alert-info hr {
  border-top-color: #a6e1ec;
}
.alert-info .alert-link {
  color: #245269;
}
.alert-warning {
  background-color: #fcf8e3;
  border-color: #faebcc;
  color: #8a6d3b;
}
.alert-warning hr {
  border-top-color: #f7e1b5;
}
.alert-warning .alert-link {
  color: #66512c;
}
.alert-danger {
  background-color: #f2dede;
  border-color: #ebccd1;
  color: #a94442;
}
.alert-danger hr {
  border-top-color: #e4b9c0;
}
.alert-danger .alert-link {
  color: #843534;
}
@-webkit-keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
@keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
.progress {
  overflow: hidden;
  height: 18px;
  margin-bottom: 18px;
  background-color: #f5f5f5;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
}
.progress-bar {
  float: left;
  width: 0%;
  height: 100%;
  font-size: 12px;
  line-height: 18px;
  color: #fff;
  text-align: center;
  background-color: #337ab7;
  -webkit-box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  -webkit-transition: width 0.6s ease;
  -o-transition: width 0.6s ease;
  transition: width 0.6s ease;
}
.progress-striped .progress-bar,
.progress-bar-striped {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-size: 40px 40px;
}
.progress.active .progress-bar,
.progress-bar.active {
  -webkit-animation: progress-bar-stripes 2s linear infinite;
  -o-animation: progress-bar-stripes 2s linear infinite;
  animation: progress-bar-stripes 2s linear infinite;
}
.progress-bar-success {
  background-color: #5cb85c;
}
.progress-striped .progress-bar-success {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-info {
  background-color: #5bc0de;
}
.progress-striped .progress-bar-info {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-warning {
  background-color: #f0ad4e;
}
.progress-striped .progress-bar-warning {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-danger {
  background-color: #d9534f;
}
.progress-striped .progress-bar-danger {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.media {
  margin-top: 15px;
}
.media:first-child {
  margin-top: 0;
}
.media,
.media-body {
  zoom: 1;
  overflow: hidden;
}
.media-body {
  width: 10000px;
}
.media-object {
  display: block;
}
.media-object.img-thumbnail {
  max-width: none;
}
.media-right,
.media > .pull-right {
  padding-left: 10px;
}
.media-left,
.media > .pull-left {
  padding-right: 10px;
}
.media-left,
.media-right,
.media-body {
  display: table-cell;
  vertical-align: top;
}
.media-middle {
  vertical-align: middle;
}
.media-bottom {
  vertical-align: bottom;
}
.media-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.media-list {
  padding-left: 0;
  list-style: none;
}
.list-group {
  margin-bottom: 20px;
  padding-left: 0;
}
.list-group-item {
  position: relative;
  display: block;
  padding: 10px 15px;
  margin-bottom: -1px;
  background-color: #fff;
  border: 1px solid #ddd;
}
.list-group-item:first-child {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
}
.list-group-item:last-child {
  margin-bottom: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
a.list-group-item,
button.list-group-item {
  color: #555;
}
a.list-group-item .list-group-item-heading,
button.list-group-item .list-group-item-heading {
  color: #333;
}
a.list-group-item:hover,
button.list-group-item:hover,
a.list-group-item:focus,
button.list-group-item:focus {
  text-decoration: none;
  color: #555;
  background-color: #f5f5f5;
}
button.list-group-item {
  width: 100%;
  text-align: left;
}
.list-group-item.disabled,
.list-group-item.disabled:hover,
.list-group-item.disabled:focus {
  background-color: #eeeeee;
  color: #777777;
  cursor: not-allowed;
}
.list-group-item.disabled .list-group-item-heading,
.list-group-item.disabled:hover .list-group-item-heading,
.list-group-item.disabled:focus .list-group-item-heading {
  color: inherit;
}
.list-group-item.disabled .list-group-item-text,
.list-group-item.disabled:hover .list-group-item-text,
.list-group-item.disabled:focus .list-group-item-text {
  color: #777777;
}
.list-group-item.active,
.list-group-item.active:hover,
.list-group-item.active:focus {
  z-index: 2;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.list-group-item.active .list-group-item-heading,
.list-group-item.active:hover .list-group-item-heading,
.list-group-item.active:focus .list-group-item-heading,
.list-group-item.active .list-group-item-heading > small,
.list-group-item.active:hover .list-group-item-heading > small,
.list-group-item.active:focus .list-group-item-heading > small,
.list-group-item.active .list-group-item-heading > .small,
.list-group-item.active:hover .list-group-item-heading > .small,
.list-group-item.active:focus .list-group-item-heading > .small {
  color: inherit;
}
.list-group-item.active .list-group-item-text,
.list-group-item.active:hover .list-group-item-text,
.list-group-item.active:focus .list-group-item-text {
  color: #c7ddef;
}
.list-group-item-success {
  color: #3c763d;
  background-color: #dff0d8;
}
a.list-group-item-success,
button.list-group-item-success {
  color: #3c763d;
}
a.list-group-item-success .list-group-item-heading,
button.list-group-item-success .list-group-item-heading {
  color: inherit;
}
a.list-group-item-success:hover,
button.list-group-item-success:hover,
a.list-group-item-success:focus,
button.list-group-item-success:focus {
  color: #3c763d;
  background-color: #d0e9c6;
}
a.list-group-item-success.active,
button.list-group-item-success.active,
a.list-group-item-success.active:hover,
button.list-group-item-success.active:hover,
a.list-group-item-success.active:focus,
button.list-group-item-success.active:focus {
  color: #fff;
  background-color: #3c763d;
  border-color: #3c763d;
}
.list-group-item-info {
  color: #31708f;
  background-color: #d9edf7;
}
a.list-group-item-info,
button.list-group-item-info {
  color: #31708f;
}
a.list-group-item-info .list-group-item-heading,
button.list-group-item-info .list-group-item-heading {
  color: inherit;
}
a.list-group-item-info:hover,
button.list-group-item-info:hover,
a.list-group-item-info:focus,
button.list-group-item-info:focus {
  color: #31708f;
  background-color: #c4e3f3;
}
a.list-group-item-info.active,
button.list-group-item-info.active,
a.list-group-item-info.active:hover,
button.list-group-item-info.active:hover,
a.list-group-item-info.active:focus,
button.list-group-item-info.active:focus {
  color: #fff;
  background-color: #31708f;
  border-color: #31708f;
}
.list-group-item-warning {
  color: #8a6d3b;
  background-color: #fcf8e3;
}
a.list-group-item-warning,
button.list-group-item-warning {
  color: #8a6d3b;
}
a.list-group-item-warning .list-group-item-heading,
button.list-group-item-warning .list-group-item-heading {
  color: inherit;
}
a.list-group-item-warning:hover,
button.list-group-item-warning:hover,
a.list-group-item-warning:focus,
button.list-group-item-warning:focus {
  color: #8a6d3b;
  background-color: #faf2cc;
}
a.list-group-item-warning.active,
button.list-group-item-warning.active,
a.list-group-item-warning.active:hover,
button.list-group-item-warning.active:hover,
a.list-group-item-warning.active:focus,
button.list-group-item-warning.active:focus {
  color: #fff;
  background-color: #8a6d3b;
  border-color: #8a6d3b;
}
.list-group-item-danger {
  color: #a94442;
  background-color: #f2dede;
}
a.list-group-item-danger,
button.list-group-item-danger {
  color: #a94442;
}
a.list-group-item-danger .list-group-item-heading,
button.list-group-item-danger .list-group-item-heading {
  color: inherit;
}
a.list-group-item-danger:hover,
button.list-group-item-danger:hover,
a.list-group-item-danger:focus,
button.list-group-item-danger:focus {
  color: #a94442;
  background-color: #ebcccc;
}
a.list-group-item-danger.active,
button.list-group-item-danger.active,
a.list-group-item-danger.active:hover,
button.list-group-item-danger.active:hover,
a.list-group-item-danger.active:focus,
button.list-group-item-danger.active:focus {
  color: #fff;
  background-color: #a94442;
  border-color: #a94442;
}
.list-group-item-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.list-group-item-text {
  margin-bottom: 0;
  line-height: 1.3;
}

.panel-body {
  padding: 15px;
}
.panel-heading {
  padding: 10px 15px;
  border-bottom: 1px solid transparent;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel-heading > .dropdown .dropdown-toggle {
  color: inherit;
}
.panel-title {
  margin-top: 0;
  margin-bottom: 0;
  font-size: 15px;
  color: inherit;
}
.panel-title > a,
.panel-title > small,
.panel-title > .small,
.panel-title > small > a,
.panel-title > .small > a {
  color: inherit;
}
.panel-footer {
  padding: 10px 15px;
  background-color: #f5f5f5;
  border-top: 1px solid #ddd;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .list-group,
.panel > .panel-collapse > .list-group {
  margin-bottom: 0;
}
.panel > .list-group .list-group-item,
.panel > .panel-collapse > .list-group .list-group-item {
  border-width: 1px 0;
  border-radius: 0;
}
.panel > .list-group:first-child .list-group-item:first-child,
.panel > .panel-collapse > .list-group:first-child .list-group-item:first-child {
  border-top: 0;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .list-group:last-child .list-group-item:last-child,
.panel > .panel-collapse > .list-group:last-child .list-group-item:last-child {
  border-bottom: 0;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .panel-heading + .panel-collapse > .list-group .list-group-item:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.panel-heading + .list-group .list-group-item:first-child {
  border-top-width: 0;
}
.list-group + .panel-footer {
  border-top-width: 0;
}
.panel > .table,
.panel > .table-responsive > .table,
.panel > .panel-collapse > .table {
  margin-bottom: 0;
}
.panel > .table caption,
.panel > .table-responsive > .table caption,
.panel > .panel-collapse > .table caption {
  padding-left: 15px;
  padding-right: 15px;
}
.panel > .table:first-child,
.panel > .table-responsive:first-child > .table:first-child {
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child {
  border-top-left-radius: 1px;
  border-top-right-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:first-child {
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:last-child {
  border-top-right-radius: 1px;
}
.panel > .table:last-child,
.panel > .table-responsive:last-child > .table:last-child {
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child {
  border-bottom-left-radius: 1px;
  border-bottom-right-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:first-child {
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:last-child {
  border-bottom-right-radius: 1px;
}
.panel > .panel-body + .table,
.panel > .panel-body + .table-responsive,
.panel > .table + .panel-body,
.panel > .table-responsive + .panel-body {
  border-top: 1px solid #ddd;
}
.panel > .table > tbody:first-child > tr:first-child th,
.panel > .table > tbody:first-child > tr:first-child td {
  border-top: 0;
}
.panel > .table-bordered,
.panel > .table-responsive > .table-bordered {
  border: 0;
}
.panel > .table-bordered > thead > tr > th:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:first-child,
.panel > .table-bordered > tbody > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:first-child,
.panel > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-bordered > thead > tr > td:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:first-child,
.panel > .table-bordered > tbody > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:first-child,
.panel > .table-bordered > tfoot > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:first-child {
  border-left: 0;
}
.panel > .table-bordered > thead > tr > th:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:last-child,
.panel > .table-bordered > tbody > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:last-child,
.panel > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-bordered > thead > tr > td:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:last-child,
.panel > .table-bordered > tbody > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:last-child,
.panel > .table-bordered > tfoot > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:last-child {
  border-right: 0;
}
.panel > .table-bordered > thead > tr:first-child > td,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > td,
.panel > .table-bordered > tbody > tr:first-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > td,
.panel > .table-bordered > thead > tr:first-child > th,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > th,
.panel > .table-bordered > tbody > tr:first-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > th {
  border-bottom: 0;
}
.panel > .table-bordered > tbody > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > td,
.panel > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-bordered > tbody > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > th,
.panel > .table-bordered > tfoot > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > th {
  border-bottom: 0;
}
.panel > .table-responsive {
  border: 0;
  margin-bottom: 0;
}
.panel-group {
  margin-bottom: 18px;
}
.panel-group .panel {
  margin-bottom: 0;
  border-radius: 2px;
}
.panel-group .panel + .panel {
  margin-top: 5px;
}
.panel-group .panel-heading {
  border-bottom: 0;
}
.panel-group .panel-heading + .panel-collapse > .panel-body,
.panel-group .panel-heading + .panel-collapse > .list-group {
  border-top: 1px solid #ddd;
}
.panel-group .panel-footer {
  border-top: 0;
}
.panel-group .panel-footer + .panel-collapse .panel-body {
  border-bottom: 1px solid #ddd;
}
.panel-default {
  border-color: #ddd;
}
.panel-default > .panel-heading {
  color: #333333;
  background-color: #f5f5f5;
  border-color: #ddd;
}
.panel-default > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ddd;
}
.panel-default > .panel-heading .badge {
  color: #f5f5f5;
  background-color: #333333;
}
.panel-default > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ddd;
}
.panel-primary {
  border-color: #337ab7;
}
.panel-primary > .panel-heading {
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.panel-primary > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #337ab7;
}
.panel-primary > .panel-heading .badge {
  color: #337ab7;
  background-color: #fff;
}
.panel-primary > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #337ab7;
}
.panel-success {
  border-color: #d6e9c6;
}
.panel-success > .panel-heading {
  color: #3c763d;
  background-color: #dff0d8;
  border-color: #d6e9c6;
}
.panel-success > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #d6e9c6;
}
.panel-success > .panel-heading .badge {
  color: #dff0d8;
  background-color: #3c763d;
}
.panel-success > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #d6e9c6;
}
.panel-info {
  border-color: #bce8f1;
}
.panel-info > .panel-heading {
  color: #31708f;
  background-color: #d9edf7;
  border-color: #bce8f1;
}
.panel-info > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #bce8f1;
}
.panel-info > .panel-heading .badge {
  color: #d9edf7;
  background-color: #31708f;
}
.panel-info > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #bce8f1;
}
.panel-warning {
  border-color: #faebcc;
}
.panel-warning > .panel-heading {
  color: #8a6d3b;
  background-color: #fcf8e3;
  border-color: #faebcc;
}
.panel-warning > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #faebcc;
}
.panel-warning > .panel-heading .badge {
  color: #fcf8e3;
  background-color: #8a6d3b;
}
.panel-warning > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #faebcc;
}
.panel-danger {
  border-color: #ebccd1;
}
.panel-danger > .panel-heading {
  color: #a94442;
  background-color: #f2dede;
  border-color: #ebccd1;
}
.panel-danger > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ebccd1;
}
.panel-danger > .panel-heading .badge {
  color: #f2dede;
  background-color: #a94442;
}
.panel-danger > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ebccd1;
}
.embed-responsive {
  position: relative;
  display: block;
  height: 0;
  padding: 0;
  overflow: hidden;
}
.embed-responsive .embed-responsive-item,
.embed-responsive iframe,
.embed-responsive embed,
.embed-responsive object,
.embed-responsive video {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  height: 100%;
  width: 100%;
  border: 0;
}
.embed-responsive-16by9 {
  padding-bottom: 56.25%;
}
.embed-responsive-4by3 {
  padding-bottom: 75%;
}
.well {
  min-height: 20px;
  padding: 19px;
  margin-bottom: 20px;
  background-color: #f5f5f5;
  border: 1px solid #e3e3e3;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
}
.well blockquote {
  border-color: #ddd;
  border-color: rgba(0, 0, 0, 0.15);
}
.well-lg {
  padding: 24px;
  border-radius: 3px;
}
.well-sm {
  padding: 9px;
  border-radius: 1px;
}
.close {
  float: right;
  font-size: 19.5px;
  font-weight: bold;
  line-height: 1;
  color: #000;
  text-shadow: 0 1px 0 #fff;
  opacity: 0.2;
  filter: alpha(opacity=20);
}
.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
  opacity: 0.5;
  filter: alpha(opacity=50);
}
button.close {
  padding: 0;
  cursor: pointer;
  background: transparent;
  border: 0;
  -webkit-appearance: none;
}
.modal-open {
  overflow: hidden;
}
.modal {
  display: none;
  overflow: hidden;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1050;
  -webkit-overflow-scrolling: touch;
  outline: 0;
}
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, -25%);
  -ms-transform: translate(0, -25%);
  -o-transform: translate(0, -25%);
  transform: translate(0, -25%);
  -webkit-transition: -webkit-transform 0.3s ease-out;
  -moz-transition: -moz-transform 0.3s ease-out;
  -o-transition: -o-transform 0.3s ease-out;
  transition: transform 0.3s ease-out;
}
.modal.in .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
.modal-open .modal {
  overflow-x: hidden;
  overflow-y: auto;
}
.modal-dialog {
  position: relative;
  width: auto;
  margin: 10px;
}
.modal-content {
  position: relative;
  background-color: #fff;
  border: 1px solid #999;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  background-clip: padding-box;
  outline: 0;
}
.modal-backdrop {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1040;
  background-color: #000;
}
.modal-backdrop.fade {
  opacity: 0;
  filter: alpha(opacity=0);
}
.modal-backdrop.in {
  opacity: 0.5;
  filter: alpha(opacity=50);
}
.modal-header {
  padding: 15px;
  border-bottom: 1px solid #e5e5e5;
}
.modal-header .close {
  margin-top: -2px;
}
.modal-title {
  margin: 0;
  line-height: 1.42857143;
}
.modal-body {
  position: relative;
  padding: 15px;
}
.modal-footer {
  padding: 15px;
  text-align: right;
  border-top: 1px solid #e5e5e5;
}
.modal-footer .btn + .btn {
  margin-left: 5px;
  margin-bottom: 0;
}
.modal-footer .btn-group .btn + .btn {
  margin-left: -1px;
}
.modal-footer .btn-block + .btn-block {
  margin-left: 0;
}
.modal-scrollbar-measure {
  position: absolute;
  top: -9999px;
  width: 50px;
  height: 50px;
  overflow: scroll;
}
@media (min-width: 768px) {
  .modal-dialog {
    width: 600px;
    margin: 30px auto;
  }
  .modal-content {
    -webkit-box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
  }
  .modal-sm {
    width: 300px;
  }
}
@media (min-width: 992px) {
  .modal-lg {
    width: 900px;
  }
}
.tooltip {
  position: absolute;
  z-index: 1070;
  display: block;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 12px;
  opacity: 0;
  filter: alpha(opacity=0);
}
.tooltip.in {
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.tooltip.top {
  margin-top: -3px;
  padding: 5px 0;
}
.tooltip.right {
  margin-left: 3px;
  padding: 0 5px;
}
.tooltip.bottom {
  margin-top: 3px;
  padding: 5px 0;
}
.tooltip.left {
  margin-left: -3px;
  padding: 0 5px;
}
.tooltip-inner {
  max-width: 200px;
  padding: 3px 8px;
  color: #fff;
  text-align: center;
  background-color: #000;
  border-radius: 2px;
}
.tooltip-arrow {
  position: absolute;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.tooltip.top .tooltip-arrow {
  bottom: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-left .tooltip-arrow {
  bottom: 0;
  right: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-right .tooltip-arrow {
  bottom: 0;
  left: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.right .tooltip-arrow {
  top: 50%;
  left: 0;
  margin-top: -5px;
  border-width: 5px 5px 5px 0;
  border-right-color: #000;
}
.tooltip.left .tooltip-arrow {
  top: 50%;
  right: 0;
  margin-top: -5px;
  border-width: 5px 0 5px 5px;
  border-left-color: #000;
}
.tooltip.bottom .tooltip-arrow {
  top: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-left .tooltip-arrow {
  top: 0;
  right: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-right .tooltip-arrow {
  top: 0;
  left: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.popover {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1060;
  display: none;
  max-width: 276px;
  padding: 1px;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 13px;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}
.popover.top {
  margin-top: -10px;
}
.popover.right {
  margin-left: 10px;
}
.popover.bottom {
  margin-top: 10px;
}
.popover.left {
  margin-left: -10px;
}
.popover-title {
  margin: 0;
  padding: 8px 14px;
  font-size: 13px;
  background-color: #f7f7f7;
  border-bottom: 1px solid #ebebeb;
  border-radius: 2px 2px 0 0;
}
.popover-content {
  padding: 9px 14px;
}
.popover > .arrow,
.popover > .arrow:after {
  position: absolute;
  display: block;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.popover > .arrow {
  border-width: 11px;
}
.popover > .arrow:after {
  border-width: 10px;
  content: "";
}
.popover.top > .arrow {
  left: 50%;
  margin-left: -11px;
  border-bottom-width: 0;
  border-top-color: #999999;
  border-top-color: rgba(0, 0, 0, 0.25);
  bottom: -11px;
}
.popover.top > .arrow:after {
  content: " ";
  bottom: 1px;
  margin-left: -10px;
  border-bottom-width: 0;
  border-top-color: #fff;
}
.popover.right > .arrow {
  top: 50%;
  left: -11px;
  margin-top: -11px;
  border-left-width: 0;
  border-right-color: #999999;
  border-right-color: rgba(0, 0, 0, 0.25);
}
.popover.right > .arrow:after {
  content: " ";
  left: 1px;
  bottom: -10px;
  border-left-width: 0;
  border-right-color: #fff;
}
.popover.bottom > .arrow {
  left: 50%;
  margin-left: -11px;
  border-top-width: 0;
  border-bottom-color: #999999;
  border-bottom-color: rgba(0, 0, 0, 0.25);
  top: -11px;
}
.popover.bottom > .arrow:after {
  content: " ";
  top: 1px;
  margin-left: -10px;
  border-top-width: 0;
  border-bottom-color: #fff;
}
.popover.left > .arrow {
  top: 50%;
  right: -11px;
  margin-top: -11px;
  border-right-width: 0;
  border-left-color: #999999;
  border-left-color: rgba(0, 0, 0, 0.25);
}
.popover.left > .arrow:after {
  content: " ";
  right: 1px;
  border-right-width: 0;
  border-left-color: #fff;
  bottom: -10px;
}
.carousel {
  position: relative;
}
.carousel-inner {
  position: relative;
  overflow: hidden;
  width: 100%;
}
.carousel-inner > .item {
  display: none;
  position: relative;
  -webkit-transition: 0.6s ease-in-out left;
  -o-transition: 0.6s ease-in-out left;
  transition: 0.6s ease-in-out left;
}
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  line-height: 1;
}
@media all and (transform-3d), (-webkit-transform-3d) {
  .carousel-inner > .item {
    -webkit-transition: -webkit-transform 0.6s ease-in-out;
    -moz-transition: -moz-transform 0.6s ease-in-out;
    -o-transition: -o-transform 0.6s ease-in-out;
    transition: transform 0.6s ease-in-out;
    -webkit-backface-visibility: hidden;
    -moz-backface-visibility: hidden;
    backface-visibility: hidden;
    -webkit-perspective: 1000px;
    -moz-perspective: 1000px;
    perspective: 1000px;
  }
  .carousel-inner > .item.next,
  .carousel-inner > .item.active.right {
    -webkit-transform: translate3d(100%, 0, 0);
    transform: translate3d(100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.prev,
  .carousel-inner > .item.active.left {
    -webkit-transform: translate3d(-100%, 0, 0);
    transform: translate3d(-100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.next.left,
  .carousel-inner > .item.prev.right,
  .carousel-inner > .item.active {
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
    left: 0;
  }
}
.carousel-inner > .active,
.carousel-inner > .next,
.carousel-inner > .prev {
  display: block;
}
.carousel-inner > .active {
  left: 0;
}
.carousel-inner > .next,
.carousel-inner > .prev {
  position: absolute;
  top: 0;
  width: 100%;
}
.carousel-inner > .next {
  left: 100%;
}
.carousel-inner > .prev {
  left: -100%;
}
.carousel-inner > .next.left,
.carousel-inner > .prev.right {
  left: 0;
}
.carousel-inner > .active.left {
  left: -100%;
}
.carousel-inner > .active.right {
  left: 100%;
}
.carousel-control {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 15%;
  opacity: 0.5;
  filter: alpha(opacity=50);
  font-size: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
  background-color: rgba(0, 0, 0, 0);
}
.carousel-control.left {
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#80000000', endColorstr='#00000000', GradientType=1);
}
.carousel-control.right {
  left: auto;
  right: 0;
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#00000000', endColorstr='#80000000', GradientType=1);
}
.carousel-control:hover,
.carousel-control:focus {
  outline: 0;
  color: #fff;
  text-decoration: none;
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.carousel-control .icon-prev,
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-left,
.carousel-control .glyphicon-chevron-right {
  position: absolute;
  top: 50%;
  margin-top: -10px;
  z-index: 5;
  display: inline-block;
}
.carousel-control .icon-prev,
.carousel-control .glyphicon-chevron-left {
  left: 50%;
  margin-left: -10px;
}
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-right {
  right: 50%;
  margin-right: -10px;
}
.carousel-control .icon-prev,
.carousel-control .icon-next {
  width: 20px;
  height: 20px;
  line-height: 1;
  font-family: serif;
}
.carousel-control .icon-prev:before {
  content: '\2039';
}
.carousel-control .icon-next:before {
  content: '\203a';
}
.carousel-indicators {
  position: absolute;
  bottom: 10px;
  left: 50%;
  z-index: 15;
  width: 60%;
  margin-left: -30%;
  padding-left: 0;
  list-style: none;
  text-align: center;
}
.carousel-indicators li {
  display: inline-block;
  width: 10px;
  height: 10px;
  margin: 1px;
  text-indent: -999px;
  border: 1px solid #fff;
  border-radius: 10px;
  cursor: pointer;
  background-color: #000 \9;
  background-color: rgba(0, 0, 0, 0);
}
.carousel-indicators .active {
  margin: 0;
  width: 12px;
  height: 12px;
  background-color: #fff;
}
.carousel-caption {
  position: absolute;
  left: 15%;
  right: 15%;
  bottom: 20px;
  z-index: 10;
  padding-top: 20px;
  padding-bottom: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
}
.carousel-caption .btn {
  text-shadow: none;
}
@media screen and (min-width: 768px) {
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-prev,
  .carousel-control .icon-next {
    width: 30px;
    height: 30px;
    margin-top: -10px;
    font-size: 30px;
  }
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .icon-prev {
    margin-left: -10px;
  }
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-next {
    margin-right: -10px;
  }
  .carousel-caption {
    left: 20%;
    right: 20%;
    padding-bottom: 30px;
  }
  .carousel-indicators {
    bottom: 20px;
  }
}
.clearfix:before,
.clearfix:after,
.dl-horizontal dd:before,
.dl-horizontal dd:after,
.container:before,
.container:after,
.container-fluid:before,
.container-fluid:after,
.row:before,
.row:after,
.form-horizontal .form-group:before,
.form-horizontal .form-group:after,
.btn-toolbar:before,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:before,
.btn-group-vertical > .btn-group:after,
.nav:before,
.nav:after,
.navbar:before,
.navbar:after,
.navbar-header:before,
.navbar-header:after,
.navbar-collapse:before,
.navbar-collapse:after,
.pager:before,
.pager:after,
.panel-body:before,
.panel-body:after,
.modal-header:before,
.modal-header:after,
.modal-footer:before,
.modal-footer:after,
.item_buttons:before,
.item_buttons:after {
  content: " ";
  display: table;
}
.clearfix:after,
.dl-horizontal dd:after,
.container:after,
.container-fluid:after,
.row:after,
.form-horizontal .form-group:after,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:after,
.nav:after,
.navbar:after,
.navbar-header:after,
.navbar-collapse:after,
.pager:after,
.panel-body:after,
.modal-header:after,
.modal-footer:after,
.item_buttons:after {
  clear: both;
}
.center-block {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.pull-right {
  float: right !important;
}
.pull-left {
  float: left !important;
}
.hide {
  display: none !important;
}
.show {
  display: block !important;
}
.invisible {
  visibility: hidden;
}
.text-hide {
  font: 0/0 a;
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}
.hidden {
  display: none !important;
}
.affix {
  position: fixed;
}
@-ms-viewport {
  width: device-width;
}
.visible-xs,
.visible-sm,
.visible-md,
.visible-lg {
  display: none !important;
}
.visible-xs-block,
.visible-xs-inline,
.visible-xs-inline-block,
.visible-sm-block,
.visible-sm-inline,
.visible-sm-inline-block,
.visible-md-block,
.visible-md-inline,
.visible-md-inline-block,
.visible-lg-block,
.visible-lg-inline,
.visible-lg-inline-block {
  display: none !important;
}
@media (max-width: 767px) {
  .visible-xs {
    display: block !important;
  }
  table.visible-xs {
    display: table !important;
  }
  tr.visible-xs {
    display: table-row !important;
  }
  th.visible-xs,
  td.visible-xs {
    display: table-cell !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-block {
    display: block !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline {
    display: inline !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm {
    display: block !important;
  }
  table.visible-sm {
    display: table !important;
  }
  tr.visible-sm {
    display: table-row !important;
  }
  th.visible-sm,
  td.visible-sm {
    display: table-cell !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-block {
    display: block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline {
    display: inline !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md {
    display: block !important;
  }
  table.visible-md {
    display: table !important;
  }
  tr.visible-md {
    display: table-row !important;
  }
  th.visible-md,
  td.visible-md {
    display: table-cell !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-block {
    display: block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline {
    display: inline !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg {
    display: block !important;
  }
  table.visible-lg {
    display: table !important;
  }
  tr.visible-lg {
    display: table-row !important;
  }
  th.visible-lg,
  td.visible-lg {
    display: table-cell !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-block {
    display: block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline {
    display: inline !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline-block {
    display: inline-block !important;
  }
}
@media (max-width: 767px) {
  .hidden-xs {
    display: none !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .hidden-sm {
    display: none !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .hidden-md {
    display: none !important;
  }
}
@media (min-width: 1200px) {
  .hidden-lg {
    display: none !important;
  }
}
.visible-print {
  display: none !important;
}
@media print {
  .visible-print {
    display: block !important;
  }
  table.visible-print {
    display: table !important;
  }
  tr.visible-print {
    display: table-row !important;
  }
  th.visible-print,
  td.visible-print {
    display: table-cell !important;
  }
}
.visible-print-block {
  display: none !important;
}
@media print {
  .visible-print-block {
    display: block !important;
  }
}
.visible-print-inline {
  display: none !important;
}
@media print {
  .visible-print-inline {
    display: inline !important;
  }
}
.visible-print-inline-block {
  display: none !important;
}
@media print {
  .visible-print-inline-block {
    display: inline-block !important;
  }
}
@media print {
  .hidden-print {
    display: none !important;
  }
}
/*!
*
* Font Awesome
*
*/
/*!
 *  Font Awesome 4.2.0 by @davegandy - http://fontawesome.io - @fontawesome
 *  License - http://fontawesome.io/license (Font: SIL OFL 1.1, CSS: MIT License)
 */
/* FONT PATH
 * -------------------------- */
@font-face {
  font-family: 'FontAwesome';
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?v=4.2.0');
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?#iefix&v=4.2.0') format('embedded-opentype'), url('../components/font-awesome/fonts/fontawesome-webfont.woff?v=4.2.0') format('woff'), url('../components/font-awesome/fonts/fontawesome-webfont.ttf?v=4.2.0') format('truetype'), url('../components/font-awesome/fonts/fontawesome-webfont.svg?v=4.2.0#fontawesomeregular') format('svg');
  font-weight: normal;
  font-style: normal;
}
.fa {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
/* makes the font 33% larger relative to the icon container */
.fa-lg {
  font-size: 1.33333333em;
  line-height: 0.75em;
  vertical-align: -15%;
}
.fa-2x {
  font-size: 2em;
}
.fa-3x {
  font-size: 3em;
}
.fa-4x {
  font-size: 4em;
}
.fa-5x {
  font-size: 5em;
}
.fa-fw {
  width: 1.28571429em;
  text-align: center;
}
.fa-ul {
  padding-left: 0;
  margin-left: 2.14285714em;
  list-style-type: none;
}
.fa-ul > li {
  position: relative;
}
.fa-li {
  position: absolute;
  left: -2.14285714em;
  width: 2.14285714em;
  top: 0.14285714em;
  text-align: center;
}
.fa-li.fa-lg {
  left: -1.85714286em;
}
.fa-border {
  padding: .2em .25em .15em;
  border: solid 0.08em #eee;
  border-radius: .1em;
}
.pull-right {
  float: right;
}
.pull-left {
  float: left;
}
.fa.pull-left {
  margin-right: .3em;
}
.fa.pull-right {
  margin-left: .3em;
}
.fa-spin {
  -webkit-animation: fa-spin 2s infinite linear;
  animation: fa-spin 2s infinite linear;
}
@-webkit-keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
@keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
.fa-rotate-90 {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=1);
  -webkit-transform: rotate(90deg);
  -ms-transform: rotate(90deg);
  transform: rotate(90deg);
}
.fa-rotate-180 {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=2);
  -webkit-transform: rotate(180deg);
  -ms-transform: rotate(180deg);
  transform: rotate(180deg);
}
.fa-rotate-270 {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=3);
  -webkit-transform: rotate(270deg);
  -ms-transform: rotate(270deg);
  transform: rotate(270deg);
}
.fa-flip-horizontal {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=0, mirror=1);
  -webkit-transform: scale(-1, 1);
  -ms-transform: scale(-1, 1);
  transform: scale(-1, 1);
}
.fa-flip-vertical {
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=2, mirror=1);
  -webkit-transform: scale(1, -1);
  -ms-transform: scale(1, -1);
  transform: scale(1, -1);
}
:root .fa-rotate-90,
:root .fa-rotate-180,
:root .fa-rotate-270,
:root .fa-flip-horizontal,
:root .fa-flip-vertical {
  filter: none;
}
.fa-stack {
  position: relative;
  display: inline-block;
  width: 2em;
  height: 2em;
  line-height: 2em;
  vertical-align: middle;
}
.fa-stack-1x,
.fa-stack-2x {
  position: absolute;
  left: 0;
  width: 100%;
  text-align: center;
}
.fa-stack-1x {
  line-height: inherit;
}
.fa-stack-2x {
  font-size: 2em;
}
.fa-inverse {
  color: #fff;
}
/* Font Awesome uses the Unicode Private Use Area (PUA) to ensure screen
   readers do not read off random characters that represent icons */
.fa-glass:before {
  content: "\f000";
}
.fa-music:before {
  content: "\f001";
}
.fa-search:before {
  content: "\f002";
}
.fa-envelope-o:before {
  content: "\f003";
}
.fa-heart:before {
  content: "\f004";
}
.fa-star:before {
  content: "\f005";
}
.fa-star-o:before {
  content: "\f006";
}
.fa-user:before {
  content: "\f007";
}
.fa-film:before {
  content: "\f008";
}
.fa-th-large:before {
  content: "\f009";
}
.fa-th:before {
  content: "\f00a";
}
.fa-th-list:before {
  content: "\f00b";
}
.fa-check:before {
  content: "\f00c";
}
.fa-remove:before,
.fa-close:before,
.fa-times:before {
  content: "\f00d";
}
.fa-search-plus:before {
  content: "\f00e";
}
.fa-search-minus:before {
  content: "\f010";
}
.fa-power-off:before {
  content: "\f011";
}
.fa-signal:before {
  content: "\f012";
}
.fa-gear:before,
.fa-cog:before {
  content: "\f013";
}
.fa-trash-o:before {
  content: "\f014";
}
.fa-home:before {
  content: "\f015";
}
.fa-file-o:before {
  content: "\f016";
}
.fa-clock-o:before {
  content: "\f017";
}
.fa-road:before {
  content: "\f018";
}
.fa-download:before {
  content: "\f019";
}
.fa-arrow-circle-o-down:before {
  content: "\f01a";
}
.fa-arrow-circle-o-up:before {
  content: "\f01b";
}
.fa-inbox:before {
  content: "\f01c";
}
.fa-play-circle-o:before {
  content: "\f01d";
}
.fa-rotate-right:before,
.fa-repeat:before {
  content: "\f01e";
}
.fa-refresh:before {
  content: "\f021";
}
.fa-list-alt:before {
  content: "\f022";
}
.fa-lock:before {
  content: "\f023";
}
.fa-flag:before {
  content: "\f024";
}
.fa-headphones:before {
  content: "\f025";
}
.fa-volume-off:before {
  content: "\f026";
}
.fa-volume-down:before {
  content: "\f027";
}
.fa-volume-up:before {
  content: "\f028";
}
.fa-qrcode:before {
  content: "\f029";
}
.fa-barcode:before {
  content: "\f02a";
}
.fa-tag:before {
  content: "\f02b";
}
.fa-tags:before {
  content: "\f02c";
}
.fa-book:before {
  content: "\f02d";
}
.fa-bookmark:before {
  content: "\f02e";
}
.fa-print:before {
  content: "\f02f";
}
.fa-camera:before {
  content: "\f030";
}
.fa-font:before {
  content: "\f031";
}
.fa-bold:before {
  content: "\f032";
}
.fa-italic:before {
  content: "\f033";
}
.fa-text-height:before {
  content: "\f034";
}
.fa-text-width:before {
  content: "\f035";
}
.fa-align-left:before {
  content: "\f036";
}
.fa-align-center:before {
  content: "\f037";
}
.fa-align-right:before {
  content: "\f038";
}
.fa-align-justify:before {
  content: "\f039";
}
.fa-list:before {
  content: "\f03a";
}
.fa-dedent:before,
.fa-outdent:before {
  content: "\f03b";
}
.fa-indent:before {
  content: "\f03c";
}
.fa-video-camera:before {
  content: "\f03d";
}
.fa-photo:before,
.fa-image:before,
.fa-picture-o:before {
  content: "\f03e";
}
.fa-pencil:before {
  content: "\f040";
}
.fa-map-marker:before {
  content: "\f041";
}
.fa-adjust:before {
  content: "\f042";
}
.fa-tint:before {
  content: "\f043";
}
.fa-edit:before,
.fa-pencil-square-o:before {
  content: "\f044";
}
.fa-share-square-o:before {
  content: "\f045";
}
.fa-check-square-o:before {
  content: "\f046";
}
.fa-arrows:before {
  content: "\f047";
}
.fa-step-backward:before {
  content: "\f048";
}
.fa-fast-backward:before {
  content: "\f049";
}
.fa-backward:before {
  content: "\f04a";
}
.fa-play:before {
  content: "\f04b";
}
.fa-pause:before {
  content: "\f04c";
}
.fa-stop:before {
  content: "\f04d";
}
.fa-forward:before {
  content: "\f04e";
}
.fa-fast-forward:before {
  content: "\f050";
}
.fa-step-forward:before {
  content: "\f051";
}
.fa-eject:before {
  content: "\f052";
}
.fa-chevron-left:before {
  content: "\f053";
}
.fa-chevron-right:before {
  content: "\f054";
}
.fa-plus-circle:before {
  content: "\f055";
}
.fa-minus-circle:before {
  content: "\f056";
}
.fa-times-circle:before {
  content: "\f057";
}
.fa-check-circle:before {
  content: "\f058";
}
.fa-question-circle:before {
  content: "\f059";
}
.fa-info-circle:before {
  content: "\f05a";
}
.fa-crosshairs:before {
  content: "\f05b";
}
.fa-times-circle-o:before {
  content: "\f05c";
}
.fa-check-circle-o:before {
  content: "\f05d";
}
.fa-ban:before {
  content: "\f05e";
}
.fa-arrow-left:before {
  content: "\f060";
}
.fa-arrow-right:before {
  content: "\f061";
}
.fa-arrow-up:before {
  content: "\f062";
}
.fa-arrow-down:before {
  content: "\f063";
}
.fa-mail-forward:before,
.fa-share:before {
  content: "\f064";
}
.fa-expand:before {
  content: "\f065";
}
.fa-compress:before {
  content: "\f066";
}
.fa-plus:before {
  content: "\f067";
}
.fa-minus:before {
  content: "\f068";
}
.fa-asterisk:before {
  content: "\f069";
}
.fa-exclamation-circle:before {
  content: "\f06a";
}
.fa-gift:before {
  content: "\f06b";
}
.fa-leaf:before {
  content: "\f06c";
}
.fa-fire:before {
  content: "\f06d";
}
.fa-eye:before {
  content: "\f06e";
}
.fa-eye-slash:before {
  content: "\f070";
}
.fa-warning:before,
.fa-exclamation-triangle:before {
  content: "\f071";
}
.fa-plane:before {
  content: "\f072";
}
.fa-calendar:before {
  content: "\f073";
}
.fa-random:before {
  content: "\f074";
}
.fa-comment:before {
  content: "\f075";
}
.fa-magnet:before {
  content: "\f076";
}
.fa-chevron-up:before {
  content: "\f077";
}
.fa-chevron-down:before {
  content: "\f078";
}
.fa-retweet:before {
  content: "\f079";
}
.fa-shopping-cart:before {
  content: "\f07a";
}
.fa-folder:before {
  content: "\f07b";
}
.fa-folder-open:before {
  content: "\f07c";
}
.fa-arrows-v:before {
  content: "\f07d";
}
.fa-arrows-h:before {
  content: "\f07e";
}
.fa-bar-chart-o:before,
.fa-bar-chart:before {
  content: "\f080";
}
.fa-twitter-square:before {
  content: "\f081";
}
.fa-facebook-square:before {
  content: "\f082";
}
.fa-camera-retro:before {
  content: "\f083";
}
.fa-key:before {
  content: "\f084";
}
.fa-gears:before,
.fa-cogs:before {
  content: "\f085";
}
.fa-comments:before {
  content: "\f086";
}
.fa-thumbs-o-up:before {
  content: "\f087";
}
.fa-thumbs-o-down:before {
  content: "\f088";
}
.fa-star-half:before {
  content: "\f089";
}
.fa-heart-o:before {
  content: "\f08a";
}
.fa-sign-out:before {
  content: "\f08b";
}
.fa-linkedin-square:before {
  content: "\f08c";
}
.fa-thumb-tack:before {
  content: "\f08d";
}
.fa-external-link:before {
  content: "\f08e";
}
.fa-sign-in:before {
  content: "\f090";
}
.fa-trophy:before {
  content: "\f091";
}
.fa-github-square:before {
  content: "\f092";
}
.fa-upload:before {
  content: "\f093";
}
.fa-lemon-o:before {
  content: "\f094";
}
.fa-phone:before {
  content: "\f095";
}
.fa-square-o:before {
  content: "\f096";
}
.fa-bookmark-o:before {
  content: "\f097";
}
.fa-phone-square:before {
  content: "\f098";
}
.fa-twitter:before {
  content: "\f099";
}
.fa-facebook:before {
  content: "\f09a";
}
.fa-github:before {
  content: "\f09b";
}
.fa-unlock:before {
  content: "\f09c";
}
.fa-credit-card:before {
  content: "\f09d";
}
.fa-rss:before {
  content: "\f09e";
}
.fa-hdd-o:before {
  content: "\f0a0";
}
.fa-bullhorn:before {
  content: "\f0a1";
}
.fa-bell:before {
  content: "\f0f3";
}
.fa-certificate:before {
  content: "\f0a3";
}
.fa-hand-o-right:before {
  content: "\f0a4";
}
.fa-hand-o-left:before {
  content: "\f0a5";
}
.fa-hand-o-up:before {
  content: "\f0a6";
}
.fa-hand-o-down:before {
  content: "\f0a7";
}
.fa-arrow-circle-left:before {
  content: "\f0a8";
}
.fa-arrow-circle-right:before {
  content: "\f0a9";
}
.fa-arrow-circle-up:before {
  content: "\f0aa";
}
.fa-arrow-circle-down:before {
  content: "\f0ab";
}
.fa-globe:before {
  content: "\f0ac";
}
.fa-wrench:before {
  content: "\f0ad";
}
.fa-tasks:before {
  content: "\f0ae";
}
.fa-filter:before {
  content: "\f0b0";
}
.fa-briefcase:before {
  content: "\f0b1";
}
.fa-arrows-alt:before {
  content: "\f0b2";
}
.fa-group:before,
.fa-users:before {
  content: "\f0c0";
}
.fa-chain:before,
.fa-link:before {
  content: "\f0c1";
}
.fa-cloud:before {
  content: "\f0c2";
}
.fa-flask:before {
  content: "\f0c3";
}
.fa-cut:before,
.fa-scissors:before {
  content: "\f0c4";
}
.fa-copy:before,
.fa-files-o:before {
  content: "\f0c5";
}
.fa-paperclip:before {
  content: "\f0c6";
}
.fa-save:before,
.fa-floppy-o:before {
  content: "\f0c7";
}
.fa-square:before {
  content: "\f0c8";
}
.fa-navicon:before,
.fa-reorder:before,
.fa-bars:before {
  content: "\f0c9";
}
.fa-list-ul:before {
  content: "\f0ca";
}
.fa-list-ol:before {
  content: "\f0cb";
}
.fa-strikethrough:before {
  content: "\f0cc";
}
.fa-underline:before {
  content: "\f0cd";
}
.fa-table:before {
  content: "\f0ce";
}
.fa-magic:before {
  content: "\f0d0";
}
.fa-truck:before {
  content: "\f0d1";
}
.fa-pinterest:before {
  content: "\f0d2";
}
.fa-pinterest-square:before {
  content: "\f0d3";
}
.fa-google-plus-square:before {
  content: "\f0d4";
}
.fa-google-plus:before {
  content: "\f0d5";
}
.fa-money:before {
  content: "\f0d6";
}
.fa-caret-down:before {
  content: "\f0d7";
}
.fa-caret-up:before {
  content: "\f0d8";
}
.fa-caret-left:before {
  content: "\f0d9";
}
.fa-caret-right:before {
  content: "\f0da";
}
.fa-columns:before {
  content: "\f0db";
}
.fa-unsorted:before,
.fa-sort:before {
  content: "\f0dc";
}
.fa-sort-down:before,
.fa-sort-desc:before {
  content: "\f0dd";
}
.fa-sort-up:before,
.fa-sort-asc:before {
  content: "\f0de";
}
.fa-envelope:before {
  content: "\f0e0";
}
.fa-linkedin:before {
  content: "\f0e1";
}
.fa-rotate-left:before,
.fa-undo:before {
  content: "\f0e2";
}
.fa-legal:before,
.fa-gavel:before {
  content: "\f0e3";
}
.fa-dashboard:before,
.fa-tachometer:before {
  content: "\f0e4";
}
.fa-comment-o:before {
  content: "\f0e5";
}
.fa-comments-o:before {
  content: "\f0e6";
}
.fa-flash:before,
.fa-bolt:before {
  content: "\f0e7";
}
.fa-sitemap:before {
  content: "\f0e8";
}
.fa-umbrella:before {
  content: "\f0e9";
}
.fa-paste:before,
.fa-clipboard:before {
  content: "\f0ea";
}
.fa-lightbulb-o:before {
  content: "\f0eb";
}
.fa-exchange:before {
  content: "\f0ec";
}
.fa-cloud-download:before {
  content: "\f0ed";
}
.fa-cloud-upload:before {
  content: "\f0ee";
}
.fa-user-md:before {
  content: "\f0f0";
}
.fa-stethoscope:before {
  content: "\f0f1";
}
.fa-suitcase:before {
  content: "\f0f2";
}
.fa-bell-o:before {
  content: "\f0a2";
}
.fa-coffee:before {
  content: "\f0f4";
}
.fa-cutlery:before {
  content: "\f0f5";
}
.fa-file-text-o:before {
  content: "\f0f6";
}
.fa-building-o:before {
  content: "\f0f7";
}
.fa-hospital-o:before {
  content: "\f0f8";
}
.fa-ambulance:before {
  content: "\f0f9";
}
.fa-medkit:before {
  content: "\f0fa";
}
.fa-fighter-jet:before {
  content: "\f0fb";
}
.fa-beer:before {
  content: "\f0fc";
}
.fa-h-square:before {
  content: "\f0fd";
}
.fa-plus-square:before {
  content: "\f0fe";
}
.fa-angle-double-left:before {
  content: "\f100";
}
.fa-angle-double-right:before {
  content: "\f101";
}
.fa-angle-double-up:before {
  content: "\f102";
}
.fa-angle-double-down:before {
  content: "\f103";
}
.fa-angle-left:before {
  content: "\f104";
}
.fa-angle-right:before {
  content: "\f105";
}
.fa-angle-up:before {
  content: "\f106";
}
.fa-angle-down:before {
  content: "\f107";
}
.fa-desktop:before {
  content: "\f108";
}
.fa-laptop:before {
  content: "\f109";
}
.fa-tablet:before {
  content: "\f10a";
}
.fa-mobile-phone:before,
.fa-mobile:before {
  content: "\f10b";
}
.fa-circle-o:before {
  content: "\f10c";
}
.fa-quote-left:before {
  content: "\f10d";
}
.fa-quote-right:before {
  content: "\f10e";
}
.fa-spinner:before {
  content: "\f110";
}
.fa-circle:before {
  content: "\f111";
}
.fa-mail-reply:before,
.fa-reply:before {
  content: "\f112";
}
.fa-github-alt:before {
  content: "\f113";
}
.fa-folder-o:before {
  content: "\f114";
}
.fa-folder-open-o:before {
  content: "\f115";
}
.fa-smile-o:before {
  content: "\f118";
}
.fa-frown-o:before {
  content: "\f119";
}
.fa-meh-o:before {
  content: "\f11a";
}
.fa-gamepad:before {
  content: "\f11b";
}
.fa-keyboard-o:before {
  content: "\f11c";
}
.fa-flag-o:before {
  content: "\f11d";
}
.fa-flag-checkered:before {
  content: "\f11e";
}
.fa-terminal:before {
  content: "\f120";
}
.fa-code:before {
  content: "\f121";
}
.fa-mail-reply-all:before,
.fa-reply-all:before {
  content: "\f122";
}
.fa-star-half-empty:before,
.fa-star-half-full:before,
.fa-star-half-o:before {
  content: "\f123";
}
.fa-location-arrow:before {
  content: "\f124";
}
.fa-crop:before {
  content: "\f125";
}
.fa-code-fork:before {
  content: "\f126";
}
.fa-unlink:before,
.fa-chain-broken:before {
  content: "\f127";
}
.fa-question:before {
  content: "\f128";
}
.fa-info:before {
  content: "\f129";
}
.fa-exclamation:before {
  content: "\f12a";
}
.fa-superscript:before {
  content: "\f12b";
}
.fa-subscript:before {
  content: "\f12c";
}
.fa-eraser:before {
  content: "\f12d";
}
.fa-puzzle-piece:before {
  content: "\f12e";
}
.fa-microphone:before {
  content: "\f130";
}
.fa-microphone-slash:before {
  content: "\f131";
}
.fa-shield:before {
  content: "\f132";
}
.fa-calendar-o:before {
  content: "\f133";
}
.fa-fire-extinguisher:before {
  content: "\f134";
}
.fa-rocket:before {
  content: "\f135";
}
.fa-maxcdn:before {
  content: "\f136";
}
.fa-chevron-circle-left:before {
  content: "\f137";
}
.fa-chevron-circle-right:before {
  content: "\f138";
}
.fa-chevron-circle-up:before {
  content: "\f139";
}
.fa-chevron-circle-down:before {
  content: "\f13a";
}
.fa-html5:before {
  content: "\f13b";
}
.fa-css3:before {
  content: "\f13c";
}
.fa-anchor:before {
  content: "\f13d";
}
.fa-unlock-alt:before {
  content: "\f13e";
}
.fa-bullseye:before {
  content: "\f140";
}
.fa-ellipsis-h:before {
  content: "\f141";
}
.fa-ellipsis-v:before {
  content: "\f142";
}
.fa-rss-square:before {
  content: "\f143";
}
.fa-play-circle:before {
  content: "\f144";
}
.fa-ticket:before {
  content: "\f145";
}
.fa-minus-square:before {
  content: "\f146";
}
.fa-minus-square-o:before {
  content: "\f147";
}
.fa-level-up:before {
  content: "\f148";
}
.fa-level-down:before {
  content: "\f149";
}
.fa-check-square:before {
  content: "\f14a";
}
.fa-pencil-square:before {
  content: "\f14b";
}
.fa-external-link-square:before {
  content: "\f14c";
}
.fa-share-square:before {
  content: "\f14d";
}
.fa-compass:before {
  content: "\f14e";
}
.fa-toggle-down:before,
.fa-caret-square-o-down:before {
  content: "\f150";
}
.fa-toggle-up:before,
.fa-caret-square-o-up:before {
  content: "\f151";
}
.fa-toggle-right:before,
.fa-caret-square-o-right:before {
  content: "\f152";
}
.fa-euro:before,
.fa-eur:before {
  content: "\f153";
}
.fa-gbp:before {
  content: "\f154";
}
.fa-dollar:before,
.fa-usd:before {
  content: "\f155";
}
.fa-rupee:before,
.fa-inr:before {
  content: "\f156";
}
.fa-cny:before,
.fa-rmb:before,
.fa-yen:before,
.fa-jpy:before {
  content: "\f157";
}
.fa-ruble:before,
.fa-rouble:before,
.fa-rub:before {
  content: "\f158";
}
.fa-won:before,
.fa-krw:before {
  content: "\f159";
}
.fa-bitcoin:before,
.fa-btc:before {
  content: "\f15a";
}
.fa-file:before {
  content: "\f15b";
}
.fa-file-text:before {
  content: "\f15c";
}
.fa-sort-alpha-asc:before {
  content: "\f15d";
}
.fa-sort-alpha-desc:before {
  content: "\f15e";
}
.fa-sort-amount-asc:before {
  content: "\f160";
}
.fa-sort-amount-desc:before {
  content: "\f161";
}
.fa-sort-numeric-asc:before {
  content: "\f162";
}
.fa-sort-numeric-desc:before {
  content: "\f163";
}
.fa-thumbs-up:before {
  content: "\f164";
}
.fa-thumbs-down:before {
  content: "\f165";
}
.fa-youtube-square:before {
  content: "\f166";
}
.fa-youtube:before {
  content: "\f167";
}
.fa-xing:before {
  content: "\f168";
}
.fa-xing-square:before {
  content: "\f169";
}
.fa-youtube-play:before {
  content: "\f16a";
}
.fa-dropbox:before {
  content: "\f16b";
}
.fa-stack-overflow:before {
  content: "\f16c";
}
.fa-instagram:before {
  content: "\f16d";
}
.fa-flickr:before {
  content: "\f16e";
}
.fa-adn:before {
  content: "\f170";
}
.fa-bitbucket:before {
  content: "\f171";
}
.fa-bitbucket-square:before {
  content: "\f172";
}
.fa-tumblr:before {
  content: "\f173";
}
.fa-tumblr-square:before {
  content: "\f174";
}
.fa-long-arrow-down:before {
  content: "\f175";
}
.fa-long-arrow-up:before {
  content: "\f176";
}
.fa-long-arrow-left:before {
  content: "\f177";
}
.fa-long-arrow-right:before {
  content: "\f178";
}
.fa-apple:before {
  content: "\f179";
}
.fa-windows:before {
  content: "\f17a";
}
.fa-android:before {
  content: "\f17b";
}
.fa-linux:before {
  content: "\f17c";
}
.fa-dribbble:before {
  content: "\f17d";
}
.fa-skype:before {
  content: "\f17e";
}
.fa-foursquare:before {
  content: "\f180";
}
.fa-trello:before {
  content: "\f181";
}
.fa-female:before {
  content: "\f182";
}
.fa-male:before {
  content: "\f183";
}
.fa-gittip:before {
  content: "\f184";
}
.fa-sun-o:before {
  content: "\f185";
}
.fa-moon-o:before {
  content: "\f186";
}
.fa-archive:before {
  content: "\f187";
}
.fa-bug:before {
  content: "\f188";
}
.fa-vk:before {
  content: "\f189";
}
.fa-weibo:before {
  content: "\f18a";
}
.fa-renren:before {
  content: "\f18b";
}
.fa-pagelines:before {
  content: "\f18c";
}
.fa-stack-exchange:before {
  content: "\f18d";
}
.fa-arrow-circle-o-right:before {
  content: "\f18e";
}
.fa-arrow-circle-o-left:before {
  content: "\f190";
}
.fa-toggle-left:before,
.fa-caret-square-o-left:before {
  content: "\f191";
}
.fa-dot-circle-o:before {
  content: "\f192";
}
.fa-wheelchair:before {
  content: "\f193";
}
.fa-vimeo-square:before {
  content: "\f194";
}
.fa-turkish-lira:before,
.fa-try:before {
  content: "\f195";
}
.fa-plus-square-o:before {
  content: "\f196";
}
.fa-space-shuttle:before {
  content: "\f197";
}
.fa-slack:before {
  content: "\f198";
}
.fa-envelope-square:before {
  content: "\f199";
}
.fa-wordpress:before {
  content: "\f19a";
}
.fa-openid:before {
  content: "\f19b";
}
.fa-institution:before,
.fa-bank:before,
.fa-university:before {
  content: "\f19c";
}
.fa-mortar-board:before,
.fa-graduation-cap:before {
  content: "\f19d";
}
.fa-yahoo:before {
  content: "\f19e";
}
.fa-google:before {
  content: "\f1a0";
}
.fa-reddit:before {
  content: "\f1a1";
}
.fa-reddit-square:before {
  content: "\f1a2";
}
.fa-stumbleupon-circle:before {
  content: "\f1a3";
}
.fa-stumbleupon:before {
  content: "\f1a4";
}
.fa-delicious:before {
  content: "\f1a5";
}
.fa-digg:before {
  content: "\f1a6";
}
.fa-pied-piper:before {
  content: "\f1a7";
}
.fa-pied-piper-alt:before {
  content: "\f1a8";
}
.fa-drupal:before {
  content: "\f1a9";
}
.fa-joomla:before {
  content: "\f1aa";
}
.fa-language:before {
  content: "\f1ab";
}
.fa-fax:before {
  content: "\f1ac";
}
.fa-building:before {
  content: "\f1ad";
}
.fa-child:before {
  content: "\f1ae";
}
.fa-paw:before {
  content: "\f1b0";
}
.fa-spoon:before {
  content: "\f1b1";
}
.fa-cube:before {
  content: "\f1b2";
}
.fa-cubes:before {
  content: "\f1b3";
}
.fa-behance:before {
  content: "\f1b4";
}
.fa-behance-square:before {
  content: "\f1b5";
}
.fa-steam:before {
  content: "\f1b6";
}
.fa-steam-square:before {
  content: "\f1b7";
}
.fa-recycle:before {
  content: "\f1b8";
}
.fa-automobile:before,
.fa-car:before {
  content: "\f1b9";
}
.fa-cab:before,
.fa-taxi:before {
  content: "\f1ba";
}
.fa-tree:before {
  content: "\f1bb";
}
.fa-spotify:before {
  content: "\f1bc";
}
.fa-deviantart:before {
  content: "\f1bd";
}
.fa-soundcloud:before {
  content: "\f1be";
}
.fa-database:before {
  content: "\f1c0";
}
.fa-file-pdf-o:before {
  content: "\f1c1";
}
.fa-file-word-o:before {
  content: "\f1c2";
}
.fa-file-excel-o:before {
  content: "\f1c3";
}
.fa-file-powerpoint-o:before {
  content: "\f1c4";
}
.fa-file-photo-o:before,
.fa-file-picture-o:before,
.fa-file-image-o:before {
  content: "\f1c5";
}
.fa-file-zip-o:before,
.fa-file-archive-o:before {
  content: "\f1c6";
}
.fa-file-sound-o:before,
.fa-file-audio-o:before {
  content: "\f1c7";
}
.fa-file-movie-o:before,
.fa-file-video-o:before {
  content: "\f1c8";
}
.fa-file-code-o:before {
  content: "\f1c9";
}
.fa-vine:before {
  content: "\f1ca";
}
.fa-codepen:before {
  content: "\f1cb";
}
.fa-jsfiddle:before {
  content: "\f1cc";
}
.fa-life-bouy:before,
.fa-life-buoy:before,
.fa-life-saver:before,
.fa-support:before,
.fa-life-ring:before {
  content: "\f1cd";
}
.fa-circle-o-notch:before {
  content: "\f1ce";
}
.fa-ra:before,
.fa-rebel:before {
  content: "\f1d0";
}
.fa-ge:before,
.fa-empire:before {
  content: "\f1d1";
}
.fa-git-square:before {
  content: "\f1d2";
}
.fa-git:before {
  content: "\f1d3";
}
.fa-hacker-news:before {
  content: "\f1d4";
}
.fa-tencent-weibo:before {
  content: "\f1d5";
}
.fa-qq:before {
  content: "\f1d6";
}
.fa-wechat:before,
.fa-weixin:before {
  content: "\f1d7";
}
.fa-send:before,
.fa-paper-plane:before {
  content: "\f1d8";
}
.fa-send-o:before,
.fa-paper-plane-o:before {
  content: "\f1d9";
}
.fa-history:before {
  content: "\f1da";
}
.fa-circle-thin:before {
  content: "\f1db";
}
.fa-header:before {
  content: "\f1dc";
}
.fa-paragraph:before {
  content: "\f1dd";
}
.fa-sliders:before {
  content: "\f1de";
}
.fa-share-alt:before {
  content: "\f1e0";
}
.fa-share-alt-square:before {
  content: "\f1e1";
}
.fa-bomb:before {
  content: "\f1e2";
}
.fa-soccer-ball-o:before,
.fa-futbol-o:before {
  content: "\f1e3";
}
.fa-tty:before {
  content: "\f1e4";
}
.fa-binoculars:before {
  content: "\f1e5";
}
.fa-plug:before {
  content: "\f1e6";
}
.fa-slideshare:before {
  content: "\f1e7";
}
.fa-twitch:before {
  content: "\f1e8";
}
.fa-yelp:before {
  content: "\f1e9";
}
.fa-newspaper-o:before {
  content: "\f1ea";
}
.fa-wifi:before {
  content: "\f1eb";
}
.fa-calculator:before {
  content: "\f1ec";
}
.fa-paypal:before {
  content: "\f1ed";
}
.fa-google-wallet:before {
  content: "\f1ee";
}
.fa-cc-visa:before {
  content: "\f1f0";
}
.fa-cc-mastercard:before {
  content: "\f1f1";
}
.fa-cc-discover:before {
  content: "\f1f2";
}
.fa-cc-amex:before {
  content: "\f1f3";
}
.fa-cc-paypal:before {
  content: "\f1f4";
}
.fa-cc-stripe:before {
  content: "\f1f5";
}
.fa-bell-slash:before {
  content: "\f1f6";
}
.fa-bell-slash-o:before {
  content: "\f1f7";
}
.fa-trash:before {
  content: "\f1f8";
}
.fa-copyright:before {
  content: "\f1f9";
}
.fa-at:before {
  content: "\f1fa";
}
.fa-eyedropper:before {
  content: "\f1fb";
}
.fa-paint-brush:before {
  content: "\f1fc";
}
.fa-birthday-cake:before {
  content: "\f1fd";
}
.fa-area-chart:before {
  content: "\f1fe";
}
.fa-pie-chart:before {
  content: "\f200";
}
.fa-line-chart:before {
  content: "\f201";
}
.fa-lastfm:before {
  content: "\f202";
}
.fa-lastfm-square:before {
  content: "\f203";
}
.fa-toggle-off:before {
  content: "\f204";
}
.fa-toggle-on:before {
  content: "\f205";
}
.fa-bicycle:before {
  content: "\f206";
}
.fa-bus:before {
  content: "\f207";
}
.fa-ioxhost:before {
  content: "\f208";
}
.fa-angellist:before {
  content: "\f209";
}
.fa-cc:before {
  content: "\f20a";
}
.fa-shekel:before,
.fa-sheqel:before,
.fa-ils:before {
  content: "\f20b";
}
.fa-meanpath:before {
  content: "\f20c";
}
/*!
*
* IPython base
*
*/
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
code {
  color: #000;
}
pre {
  font-size: inherit;
  line-height: inherit;
}
label {
  font-weight: normal;
}
/* Make the page background atleast 100% the height of the view port */
/* Make the page itself atleast 70% the height of the view port */
.border-box-sizing {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.corner-all {
  border-radius: 2px;
}
.no-padding {
  padding: 0px;
}
/* Flexible box model classes */
/* Taken from Alex Russell http://infrequently.org/2009/08/css-3-progress/ */
/* This file is a compatability layer.  It allows the usage of flexible box 
model layouts accross multiple browsers, including older browsers.  The newest,
universal implementation of the flexible box model is used when available (see
`Modern browsers` comments below).  Browsers that are known to implement this 
new spec completely include:

    Firefox 28.0+
    Chrome 29.0+
    Internet Explorer 11+ 
    Opera 17.0+

Browsers not listed, including Safari, are supported via the styling under the
`Old browsers` comments below.
*/
.hbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
.hbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.vbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
.vbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.hbox.reverse,
.vbox.reverse,
.reverse {
  /* Old browsers */
  -webkit-box-direction: reverse;
  -moz-box-direction: reverse;
  box-direction: reverse;
  /* Modern browsers */
  flex-direction: row-reverse;
}
.hbox.box-flex0,
.vbox.box-flex0,
.box-flex0 {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
  width: auto;
}
.hbox.box-flex1,
.vbox.box-flex1,
.box-flex1 {
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex,
.vbox.box-flex,
.box-flex {
  /* Old browsers */
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex2,
.vbox.box-flex2,
.box-flex2 {
  /* Old browsers */
  -webkit-box-flex: 2;
  -moz-box-flex: 2;
  box-flex: 2;
  /* Modern browsers */
  flex: 2;
}
.box-group1 {
  /*  Deprecated */
  -webkit-box-flex-group: 1;
  -moz-box-flex-group: 1;
  box-flex-group: 1;
}
.box-group2 {
  /* Deprecated */
  -webkit-box-flex-group: 2;
  -moz-box-flex-group: 2;
  box-flex-group: 2;
}
.hbox.start,
.vbox.start,
.start {
  /* Old browsers */
  -webkit-box-pack: start;
  -moz-box-pack: start;
  box-pack: start;
  /* Modern browsers */
  justify-content: flex-start;
}
.hbox.end,
.vbox.end,
.end {
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
}
.hbox.center,
.vbox.center,
.center {
  /* Old browsers */
  -webkit-box-pack: center;
  -moz-box-pack: center;
  box-pack: center;
  /* Modern browsers */
  justify-content: center;
}
.hbox.baseline,
.vbox.baseline,
.baseline {
  /* Old browsers */
  -webkit-box-pack: baseline;
  -moz-box-pack: baseline;
  box-pack: baseline;
  /* Modern browsers */
  justify-content: baseline;
}
.hbox.stretch,
.vbox.stretch,
.stretch {
  /* Old browsers */
  -webkit-box-pack: stretch;
  -moz-box-pack: stretch;
  box-pack: stretch;
  /* Modern browsers */
  justify-content: stretch;
}
.hbox.align-start,
.vbox.align-start,
.align-start {
  /* Old browsers */
  -webkit-box-align: start;
  -moz-box-align: start;
  box-align: start;
  /* Modern browsers */
  align-items: flex-start;
}
.hbox.align-end,
.vbox.align-end,
.align-end {
  /* Old browsers */
  -webkit-box-align: end;
  -moz-box-align: end;
  box-align: end;
  /* Modern browsers */
  align-items: flex-end;
}
.hbox.align-center,
.vbox.align-center,
.align-center {
  /* Old browsers */
  -webkit-box-align: center;
  -moz-box-align: center;
  box-align: center;
  /* Modern browsers */
  align-items: center;
}
.hbox.align-baseline,
.vbox.align-baseline,
.align-baseline {
  /* Old browsers */
  -webkit-box-align: baseline;
  -moz-box-align: baseline;
  box-align: baseline;
  /* Modern browsers */
  align-items: baseline;
}
.hbox.align-stretch,
.vbox.align-stretch,
.align-stretch {
  /* Old browsers */
  -webkit-box-align: stretch;
  -moz-box-align: stretch;
  box-align: stretch;
  /* Modern browsers */
  align-items: stretch;
}
div.error {
  margin: 2em;
  text-align: center;
}
div.error > h1 {
  font-size: 500%;
  line-height: normal;
}
div.error > p {
  font-size: 200%;
  line-height: normal;
}
div.traceback-wrapper {
  text-align: left;
  max-width: 800px;
  margin: auto;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
body {
  background-color: #fff;
  /* This makes sure that the body covers the entire window and needs to
       be in a different element than the display: box in wrapper below */
  position: absolute;
  left: 0px;
  right: 0px;
  top: 0px;
  bottom: 0px;
  overflow: visible;
}
body > #header {
  /* Initially hidden to prevent FLOUC */
  display: none;
  background-color: #fff;
  /* Display over codemirror */
  position: relative;
  z-index: 100;
}
body > #header #header-container {
  padding-bottom: 5px;
  padding-top: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
body > #header .header-bar {
  width: 100%;
  height: 1px;
  background: #e7e7e7;
  margin-bottom: -1px;
}
@media print {
  body > #header {
    display: none !important;
  }
}
#header-spacer {
  width: 100%;
  visibility: hidden;
}
@media print {
  #header-spacer {
    display: none;
  }
}
#ipython_notebook {
  padding-left: 0px;
  padding-top: 1px;
  padding-bottom: 1px;
}
@media (max-width: 991px) {
  #ipython_notebook {
    margin-left: 10px;
  }
}
[dir="rtl"] #ipython_notebook {
  float: right !important;
}
#noscript {
  width: auto;
  padding-top: 16px;
  padding-bottom: 16px;
  text-align: center;
  font-size: 22px;
  color: red;
  font-weight: bold;
}
#ipython_notebook img {
  height: 28px;
}
#site {
  width: 100%;
  display: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  overflow: auto;
}
@media print {
  #site {
    height: auto !important;
  }
}
/* Smaller buttons */
.ui-button .ui-button-text {
  padding: 0.2em 0.8em;
  font-size: 77%;
}
input.ui-button {
  padding: 0.3em 0.9em;
}
span#login_widget {
  float: right;
}
span#login_widget > .button,
#logout {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button:focus,
#logout:focus,
span#login_widget > .button.focus,
#logout.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
span#login_widget > .button:hover,
#logout:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active:hover,
#logout:active:hover,
span#login_widget > .button.active:hover,
#logout.active:hover,
.open > .dropdown-togglespan#login_widget > .button:hover,
.open > .dropdown-toggle#logout:hover,
span#login_widget > .button:active:focus,
#logout:active:focus,
span#login_widget > .button.active:focus,
#logout.active:focus,
.open > .dropdown-togglespan#login_widget > .button:focus,
.open > .dropdown-toggle#logout:focus,
span#login_widget > .button:active.focus,
#logout:active.focus,
span#login_widget > .button.active.focus,
#logout.active.focus,
.open > .dropdown-togglespan#login_widget > .button.focus,
.open > .dropdown-toggle#logout.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  background-image: none;
}
span#login_widget > .button.disabled:hover,
#logout.disabled:hover,
span#login_widget > .button[disabled]:hover,
#logout[disabled]:hover,
fieldset[disabled] span#login_widget > .button:hover,
fieldset[disabled] #logout:hover,
span#login_widget > .button.disabled:focus,
#logout.disabled:focus,
span#login_widget > .button[disabled]:focus,
#logout[disabled]:focus,
fieldset[disabled] span#login_widget > .button:focus,
fieldset[disabled] #logout:focus,
span#login_widget > .button.disabled.focus,
#logout.disabled.focus,
span#login_widget > .button[disabled].focus,
#logout[disabled].focus,
fieldset[disabled] span#login_widget > .button.focus,
fieldset[disabled] #logout.focus {
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button .badge,
#logout .badge {
  color: #fff;
  background-color: #333;
}
.nav-header {
  text-transform: none;
}
#header > span {
  margin-top: 10px;
}
.modal_stretch .modal-dialog {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  min-height: 80vh;
}
.modal_stretch .modal-dialog .modal-body {
  max-height: calc(100vh - 200px);
  overflow: auto;
  flex: 1;
}
@media (min-width: 768px) {
  .modal .modal-dialog {
    width: 700px;
  }
}
@media (min-width: 768px) {
  select.form-control {
    margin-left: 12px;
    margin-right: 12px;
  }
}
/*!
*
* IPython auth
*
*/
.center-nav {
  display: inline-block;
  margin-bottom: -4px;
}
/*!
*
* IPython tree view
*
*/
/* We need an invisible input field on top of the sentense*/
/* "Drag file onto the list ..." */
.alternate_upload {
  background-color: none;
  display: inline;
}
.alternate_upload.form {
  padding: 0;
  margin: 0;
}
.alternate_upload input.fileinput {
  text-align: center;
  vertical-align: middle;
  display: inline;
  opacity: 0;
  z-index: 2;
  width: 12ex;
  margin-right: -12ex;
}
.alternate_upload .btn-upload {
  height: 22px;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
[dir="rtl"] #tabs li {
  float: right;
}
ul#tabs {
  margin-bottom: 4px;
}
[dir="rtl"] ul#tabs {
  margin-right: 0px;
}
ul#tabs a {
  padding-top: 6px;
  padding-bottom: 4px;
}
ul.breadcrumb a:focus,
ul.breadcrumb a:hover {
  text-decoration: none;
}
ul.breadcrumb i.icon-home {
  font-size: 16px;
  margin-right: 4px;
}
ul.breadcrumb span {
  color: #5e5e5e;
}
.list_toolbar {
  padding: 4px 0 4px 0;
  vertical-align: middle;
}
.list_toolbar .tree-buttons {
  padding-top: 1px;
}
[dir="rtl"] .list_toolbar .tree-buttons {
  float: left !important;
}
[dir="rtl"] .list_toolbar .pull-right {
  padding-top: 1px;
  float: left !important;
}
[dir="rtl"] .list_toolbar .pull-left {
  float: right !important;
}
.dynamic-buttons {
  padding-top: 3px;
  display: inline-block;
}
.list_toolbar [class*="span"] {
  min-height: 24px;
}
.list_header {
  font-weight: bold;
  background-color: #EEE;
}
.list_placeholder {
  font-weight: bold;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
}
.list_container {
  margin-top: 4px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 2px;
}
.list_container > div {
  border-bottom: 1px solid #ddd;
}
.list_container > div:hover .list-item {
  background-color: red;
}
.list_container > div:last-child {
  border: none;
}
.list_item:hover .list_item {
  background-color: #ddd;
}
.list_item a {
  text-decoration: none;
}
.list_item:hover {
  background-color: #fafafa;
}
.list_header > div,
.list_item > div {
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
.list_header > div input,
.list_item > div input {
  margin-right: 7px;
  margin-left: 14px;
  vertical-align: baseline;
  line-height: 22px;
  position: relative;
  top: -1px;
}
.list_header > div .item_link,
.list_item > div .item_link {
  margin-left: -1px;
  vertical-align: baseline;
  line-height: 22px;
}
.new-file input[type=checkbox] {
  visibility: hidden;
}
.item_name {
  line-height: 22px;
  height: 24px;
}
.item_icon {
  font-size: 14px;
  color: #5e5e5e;
  margin-right: 7px;
  margin-left: 7px;
  line-height: 22px;
  vertical-align: baseline;
}
.item_buttons {
  line-height: 1em;
  margin-left: -5px;
}
.item_buttons .btn,
.item_buttons .btn-group,
.item_buttons .input-group {
  float: left;
}
.item_buttons > .btn,
.item_buttons > .btn-group,
.item_buttons > .input-group {
  margin-left: 5px;
}
.item_buttons .btn {
  min-width: 13ex;
}
.item_buttons .running-indicator {
  padding-top: 4px;
  color: #5cb85c;
}
.item_buttons .kernel-name {
  padding-top: 4px;
  color: #5bc0de;
  margin-right: 7px;
  float: left;
}
.toolbar_info {
  height: 24px;
  line-height: 24px;
}
.list_item input:not([type=checkbox]) {
  padding-top: 3px;
  padding-bottom: 3px;
  height: 22px;
  line-height: 14px;
  margin: 0px;
}
.highlight_text {
  color: blue;
}
#project_name {
  display: inline-block;
  padding-left: 7px;
  margin-left: -2px;
}
#project_name > .breadcrumb {
  padding: 0px;
  margin-bottom: 0px;
  background-color: transparent;
  font-weight: bold;
}
#tree-selector {
  padding-right: 0px;
}
[dir="rtl"] #tree-selector a {
  float: right;
}
#button-select-all {
  min-width: 50px;
}
#select-all {
  margin-left: 7px;
  margin-right: 2px;
}
.menu_icon {
  margin-right: 2px;
}
.tab-content .row {
  margin-left: 0px;
  margin-right: 0px;
}
.folder_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f114";
}
.folder_icon:before.pull-left {
  margin-right: .3em;
}
.folder_icon:before.pull-right {
  margin-left: .3em;
}
.notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
}
.notebook_icon:before.pull-left {
  margin-right: .3em;
}
.notebook_icon:before.pull-right {
  margin-left: .3em;
}
.running_notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
  color: #5cb85c;
}
.running_notebook_icon:before.pull-left {
  margin-right: .3em;
}
.running_notebook_icon:before.pull-right {
  margin-left: .3em;
}
.file_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f016";
  position: relative;
  top: -2px;
}
.file_icon:before.pull-left {
  margin-right: .3em;
}
.file_icon:before.pull-right {
  margin-left: .3em;
}
#notebook_toolbar .pull-right {
  padding-top: 0px;
  margin-right: -1px;
}
ul#new-menu {
  left: auto;
  right: 0;
}
[dir="rtl"] #new-menu {
  text-align: right;
}
.kernel-menu-icon {
  padding-right: 12px;
  width: 24px;
  content: "\f096";
}
.kernel-menu-icon:before {
  content: "\f096";
}
.kernel-menu-icon-current:before {
  content: "\f00c";
}
#tab_content {
  padding-top: 20px;
}
#running .panel-group .panel {
  margin-top: 3px;
  margin-bottom: 1em;
}
#running .panel-group .panel .panel-heading {
  background-color: #EEE;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
#running .panel-group .panel .panel-heading a:focus,
#running .panel-group .panel .panel-heading a:hover {
  text-decoration: none;
}
#running .panel-group .panel .panel-body {
  padding: 0px;
}
#running .panel-group .panel .panel-body .list_container {
  margin-top: 0px;
  margin-bottom: 0px;
  border: 0px;
  border-radius: 0px;
}
#running .panel-group .panel .panel-body .list_container .list_item {
  border-bottom: 1px solid #ddd;
}
#running .panel-group .panel .panel-body .list_container .list_item:last-child {
  border-bottom: 0px;
}
[dir="rtl"] #running .col-sm-8 {
  float: right !important;
}
.delete-button {
  display: none;
}
.duplicate-button {
  display: none;
}
.rename-button {
  display: none;
}
.shutdown-button {
  display: none;
}
.dynamic-instructions {
  display: inline-block;
  padding-top: 4px;
}
/*!
*
* IPython text editor webapp
*
*/
.selected-keymap i.fa {
  padding: 0px 5px;
}
.selected-keymap i.fa:before {
  content: "\f00c";
}
#mode-menu {
  overflow: auto;
  max-height: 20em;
}
.edit_app #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.edit_app #menubar .navbar {
  /* Use a negative 1 bottom margin, so the border overlaps the border of the
    header */
  margin-bottom: -1px;
}
.dirty-indicator {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator.pull-left {
  margin-right: .3em;
}
.dirty-indicator.pull-right {
  margin-left: .3em;
}
.dirty-indicator-dirty {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-dirty.pull-left {
  margin-right: .3em;
}
.dirty-indicator-dirty.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-clean.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f00c";
}
.dirty-indicator-clean:before.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean:before.pull-right {
  margin-left: .3em;
}
#filename {
  font-size: 16pt;
  display: table;
  padding: 0px 5px;
}
#current-mode {
  padding-left: 5px;
  padding-right: 5px;
}
#texteditor-backdrop {
  padding-top: 20px;
  padding-bottom: 20px;
}
@media not print {
  #texteditor-backdrop {
    background-color: #EEE;
  }
}
@media print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container {
    padding: 0px;
    background-color: #fff;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
/*!
*
* IPython notebook
*
*/
/* CSS font colors for translated ANSI colors. */
.ansibold {
  font-weight: bold;
}
/* use dark versions for foreground, to improve visibility */
.ansiblack {
  color: black;
}
.ansired {
  color: darkred;
}
.ansigreen {
  color: darkgreen;
}
.ansiyellow {
  color: #c4a000;
}
.ansiblue {
  color: darkblue;
}
.ansipurple {
  color: darkviolet;
}
.ansicyan {
  color: steelblue;
}
.ansigray {
  color: gray;
}
/* and light for background, for the same reason */
.ansibgblack {
  background-color: black;
}
.ansibgred {
  background-color: red;
}
.ansibggreen {
  background-color: green;
}
.ansibgyellow {
  background-color: yellow;
}
.ansibgblue {
  background-color: blue;
}
.ansibgpurple {
  background-color: magenta;
}
.ansibgcyan {
  background-color: cyan;
}
.ansibggray {
  background-color: gray;
}
div.cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  border-radius: 2px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  border-width: 1px;
  border-style: solid;
  border-color: transparent;
  width: 100%;
  padding: 5px;
  /* This acts as a spacer between cells, that is outside the border */
  margin: 0px;
  outline: none;
  border-left-width: 1px;
  padding-left: 5px;
  background: linear-gradient(to right, transparent -40px, transparent 1px, transparent 1px, transparent 100%);
}
div.cell.jupyter-soft-selected {
  border-left-color: #90CAF9;
  border-left-color: #E3F2FD;
  border-left-width: 1px;
  padding-left: 5px;
  border-right-color: #E3F2FD;
  border-right-width: 1px;
  background: #E3F2FD;
}
@media print {
  div.cell.jupyter-soft-selected {
    border-color: transparent;
  }
}
div.cell.selected {
  border-color: #ababab;
  border-left-width: 0px;
  padding-left: 6px;
  background: linear-gradient(to right, #42A5F5 -40px, #42A5F5 5px, transparent 5px, transparent 100%);
}
@media print {
  div.cell.selected {
    border-color: transparent;
  }
}
div.cell.selected.jupyter-soft-selected {
  border-left-width: 0;
  padding-left: 6px;
  background: linear-gradient(to right, #42A5F5 -40px, #42A5F5 7px, #E3F2FD 7px, #E3F2FD 100%);
}
.edit_mode div.cell.selected {
  border-color: #66BB6A;
  border-left-width: 0px;
  padding-left: 6px;
  background: linear-gradient(to right, #66BB6A -40px, #66BB6A 5px, transparent 5px, transparent 100%);
}
@media print {
  .edit_mode div.cell.selected {
    border-color: transparent;
  }
}
.prompt {
  /* This needs to be wide enough for 3 digit prompt numbers: In[100]: */
  min-width: 14ex;
  /* This padding is tuned to match the padding on the CodeMirror editor. */
  padding: 0.4em;
  margin: 0px;
  font-family: monospace;
  text-align: right;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
  /* Don't highlight prompt number selection */
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  /* Use default cursor */
  cursor: default;
}
@media (max-width: 540px) {
  .prompt {
    text-align: left;
  }
}
div.inner_cell {
  min-width: 0;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_area {
  border: 1px solid #cfcfcf;
  border-radius: 2px;
  background: #f7f7f7;
  line-height: 1.21429em;
}
/* This is needed so that empty prompt areas can collapse to zero height when there
   is no content in the output_subarea and the prompt. The main purpose of this is
   to make sure that empty JavaScript output_subareas have no height. */
div.prompt:empty {
  padding-top: 0;
  padding-bottom: 0;
}
div.unrecognized_cell {
  padding: 5px 5px 5px 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.unrecognized_cell .inner_cell {
  border-radius: 2px;
  padding: 5px;
  font-weight: bold;
  color: red;
  border: 1px solid #cfcfcf;
  background: #eaeaea;
}
div.unrecognized_cell .inner_cell a {
  color: inherit;
  text-decoration: none;
}
div.unrecognized_cell .inner_cell a:hover {
  color: inherit;
  text-decoration: none;
}
@media (max-width: 540px) {
  div.unrecognized_cell > div.prompt {
    display: none;
  }
}
div.code_cell {
  /* avoid page breaking on code cells when printing */
}
@media print {
  div.code_cell {
    page-break-inside: avoid;
  }
}
/* any special styling for code cells that are currently running goes here */
div.input {
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.input {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_prompt {
  color: #303F9F;
  border-top: 1px solid transparent;
}
div.input_area > div.highlight {
  margin: 0.4em;
  border: none;
  padding: 0px;
  background-color: transparent;
}
div.input_area > div.highlight > pre {
  margin: 0px;
  border: none;
  padding: 0px;
  background-color: transparent;
}
/* The following gets added to the <head> if it is detected that the user has a
 * monospace font with inconsistent normal/bold/italic height.  See
 * notebookmain.js.  Such fonts will have keywords vertically offset with
 * respect to the rest of the text.  The user should select a better font.
 * See: https://github.com/ipython/ipython/issues/1503
 *
 * .CodeMirror span {
 *      vertical-align: bottom;
 * }
 */
.CodeMirror {
  line-height: 1.21429em;
  /* Changed from 1em to our global default */
  font-size: 14px;
  height: auto;
  /* Changed to auto to autogrow */
  background: none;
  /* Changed from white to allow our bg to show through */
}
.CodeMirror-scroll {
  /*  The CodeMirror docs are a bit fuzzy on if overflow-y should be hidden or visible.*/
  /*  We have found that if it is visible, vertical scrollbars appear with font size changes.*/
  overflow-y: hidden;
  overflow-x: auto;
}
.CodeMirror-lines {
  /* In CM2, this used to be 0.4em, but in CM3 it went to 4px. We need the em value because */
  /* we have set a different line-height and want this to scale with that. */
  padding: 0.4em;
}
.CodeMirror-linenumber {
  padding: 0 8px 0 4px;
}
.CodeMirror-gutters {
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.CodeMirror pre {
  /* In CM3 this went to 4px from 0 in CM2. We need the 0 value because of how we size */
  /* .CodeMirror-lines */
  padding: 0;
  border: 0;
  border-radius: 0;
}
/*

Original style from softwaremaniacs.org (c) Ivan Sagalaev <Maniac@SoftwareManiacs.Org>
Adapted from GitHub theme

*/
.highlight-base {
  color: #000;
}
.highlight-variable {
  color: #000;
}
.highlight-variable-2 {
  color: #1a1a1a;
}
.highlight-variable-3 {
  color: #333333;
}
.highlight-string {
  color: #BA2121;
}
.highlight-comment {
  color: #408080;
  font-style: italic;
}
.highlight-number {
  color: #080;
}
.highlight-atom {
  color: #88F;
}
.highlight-keyword {
  color: #008000;
  font-weight: bold;
}
.highlight-builtin {
  color: #008000;
}
.highlight-error {
  color: #f00;
}
.highlight-operator {
  color: #AA22FF;
  font-weight: bold;
}
.highlight-meta {
  color: #AA22FF;
}
/* previously not defined, copying from default codemirror */
.highlight-def {
  color: #00f;
}
.highlight-string-2 {
  color: #f50;
}
.highlight-qualifier {
  color: #555;
}
.highlight-bracket {
  color: #997;
}
.highlight-tag {
  color: #170;
}
.highlight-attribute {
  color: #00c;
}
.highlight-header {
  color: blue;
}
.highlight-quote {
  color: #090;
}
.highlight-link {
  color: #00c;
}
/* apply the same style to codemirror */
.cm-s-ipython span.cm-keyword {
  color: #008000;
  font-weight: bold;
}
.cm-s-ipython span.cm-atom {
  color: #88F;
}
.cm-s-ipython span.cm-number {
  color: #080;
}
.cm-s-ipython span.cm-def {
  color: #00f;
}
.cm-s-ipython span.cm-variable {
  color: #000;
}
.cm-s-ipython span.cm-operator {
  color: #AA22FF;
  font-weight: bold;
}
.cm-s-ipython span.cm-variable-2 {
  color: #1a1a1a;
}
.cm-s-ipython span.cm-variable-3 {
  color: #333333;
}
.cm-s-ipython span.cm-comment {
  color: #408080;
  font-style: italic;
}
.cm-s-ipython span.cm-string {
  color: #BA2121;
}
.cm-s-ipython span.cm-string-2 {
  color: #f50;
}
.cm-s-ipython span.cm-meta {
  color: #AA22FF;
}
.cm-s-ipython span.cm-qualifier {
  color: #555;
}
.cm-s-ipython span.cm-builtin {
  color: #008000;
}
.cm-s-ipython span.cm-bracket {
  color: #997;
}
.cm-s-ipython span.cm-tag {
  color: #170;
}
.cm-s-ipython span.cm-attribute {
  color: #00c;
}
.cm-s-ipython span.cm-header {
  color: blue;
}
.cm-s-ipython span.cm-quote {
  color: #090;
}
.cm-s-ipython span.cm-link {
  color: #00c;
}
.cm-s-ipython span.cm-error {
  color: #f00;
}
.cm-s-ipython span.cm-tab {
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAMCAYAAAAkuj5RAAAAAXNSR0IArs4c6QAAAGFJREFUSMft1LsRQFAQheHPowAKoACx3IgEKtaEHujDjORSgWTH/ZOdnZOcM/sgk/kFFWY0qV8foQwS4MKBCS3qR6ixBJvElOobYAtivseIE120FaowJPN75GMu8j/LfMwNjh4HUpwg4LUAAAAASUVORK5CYII=);
  background-position: right;
  background-repeat: no-repeat;
}
div.output_wrapper {
  /* this position must be relative to enable descendents to be absolute within it */
  position: relative;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  z-index: 1;
}
/* class for the output area when it should be height-limited */
div.output_scroll {
  /* ideally, this would be max-height, but FF barfs all over that */
  height: 24em;
  /* FF needs this *and the wrapper* to specify full width, or it will shrinkwrap */
  width: 100%;
  overflow: auto;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  display: block;
}
/* output div while it is collapsed */
div.output_collapsed {
  margin: 0px;
  padding: 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
div.out_prompt_overlay {
  height: 100%;
  padding: 0px 0.4em;
  position: absolute;
  border-radius: 2px;
}
div.out_prompt_overlay:hover {
  /* use inner shadow to get border that is computed the same on WebKit/FF */
  -webkit-box-shadow: inset 0 0 1px #000;
  box-shadow: inset 0 0 1px #000;
  background: rgba(240, 240, 240, 0.5);
}
div.output_prompt {
  color: #D84315;
}
/* This class is the outer container of all output sections. */
div.output_area {
  padding: 0px;
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.output_area .MathJax_Display {
  text-align: left !important;
}
div.output_area .rendered_html table {
  margin-left: 0;
  margin-right: 0;
}
div.output_area .rendered_html img {
  margin-left: 0;
  margin-right: 0;
}
div.output_area img,
div.output_area svg {
  max-width: 100%;
  height: auto;
}
div.output_area img.unconfined,
div.output_area svg.unconfined {
  max-width: none;
}
/* This is needed to protect the pre formating from global settings such
   as that of bootstrap */
.output {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.output_area {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
div.output_area pre {
  margin: 0;
  padding: 0;
  border: 0;
  vertical-align: baseline;
  color: black;
  background-color: transparent;
  border-radius: 0;
}
/* This class is for the output subarea inside the output_area and after
   the prompt div. */
div.output_subarea {
  overflow-x: auto;
  padding: 0.4em;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
  max-width: calc(100% - 14ex);
}
div.output_scroll div.output_subarea {
  overflow-x: visible;
}
/* The rest of the output_* classes are for special styling of the different
   output types */
/* all text output has this class: */
div.output_text {
  text-align: left;
  color: #000;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
}
/* stdout/stderr are 'text' as well as 'stream', but execute_result/error are *not* streams */
div.output_stderr {
  background: #fdd;
  /* very light red background for stderr */
}
div.output_latex {
  text-align: left;
}
/* Empty output_javascript divs should have no height */
div.output_javascript:empty {
  padding: 0;
}
.js-error {
  color: darkred;
}
/* raw_input styles */
div.raw_input_container {
  line-height: 1.21429em;
  padding-top: 5px;
}
pre.raw_input_prompt {
  /* nothing needed here. */
}
input.raw_input {
  font-family: monospace;
  font-size: inherit;
  color: inherit;
  width: auto;
  /* make sure input baseline aligns with prompt */
  vertical-align: baseline;
  /* padding + margin = 0.5em between prompt and cursor */
  padding: 0em 0.25em;
  margin: 0em 0.25em;
}
input.raw_input:focus {
  box-shadow: none;
}
p.p-space {
  margin-bottom: 10px;
}
div.output_unrecognized {
  padding: 5px;
  font-weight: bold;
  color: red;
}
div.output_unrecognized a {
  color: inherit;
  text-decoration: none;
}
div.output_unrecognized a:hover {
  color: inherit;
  text-decoration: none;
}
.rendered_html {
  color: #000;
  /* any extras will just be numbers: */
}
.rendered_html em {
  font-style: italic;
}
.rendered_html strong {
  font-weight: bold;
}
.rendered_html u {
  text-decoration: underline;
}
.rendered_html :link {
  text-decoration: underline;
}
.rendered_html :visited {
  text-decoration: underline;
}
.rendered_html h1 {
  font-size: 185.7%;
  margin: 1.08em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h2 {
  font-size: 157.1%;
  margin: 1.27em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h3 {
  font-size: 128.6%;
  margin: 1.55em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h4 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h5 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h6 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h1:first-child {
  margin-top: 0.538em;
}
.rendered_html h2:first-child {
  margin-top: 0.636em;
}
.rendered_html h3:first-child {
  margin-top: 0.777em;
}
.rendered_html h4:first-child {
  margin-top: 1em;
}
.rendered_html h5:first-child {
  margin-top: 1em;
}
.rendered_html h6:first-child {
  margin-top: 1em;
}
.rendered_html ul {
  list-style: disc;
  margin: 0em 2em;
  padding-left: 0px;
}
.rendered_html ul ul {
  list-style: square;
  margin: 0em 2em;
}
.rendered_html ul ul ul {
  list-style: circle;
  margin: 0em 2em;
}
.rendered_html ol {
  list-style: decimal;
  margin: 0em 2em;
  padding-left: 0px;
}
.rendered_html ol ol {
  list-style: upper-alpha;
  margin: 0em 2em;
}
.rendered_html ol ol ol {
  list-style: lower-alpha;
  margin: 0em 2em;
}
.rendered_html ol ol ol ol {
  list-style: lower-roman;
  margin: 0em 2em;
}
.rendered_html ol ol ol ol ol {
  list-style: decimal;
  margin: 0em 2em;
}
.rendered_html * + ul {
  margin-top: 1em;
}
.rendered_html * + ol {
  margin-top: 1em;
}
.rendered_html hr {
  color: black;
  background-color: black;
}
.rendered_html pre {
  margin: 1em 2em;
}
.rendered_html pre,
.rendered_html code {
  border: 0;
  background-color: #fff;
  color: #000;
  font-size: 100%;
  padding: 0px;
}
.rendered_html blockquote {
  margin: 1em 2em;
}
.rendered_html table {
  margin-left: auto;
  margin-right: auto;
  border: 1px solid black;
  border-collapse: collapse;
}
.rendered_html tr,
.rendered_html th,
.rendered_html td {
  border: 1px solid black;
  border-collapse: collapse;
  margin: 1em 2em;
}
.rendered_html td,
.rendered_html th {
  text-align: left;
  vertical-align: middle;
  padding: 4px;
}
.rendered_html th {
  font-weight: bold;
}
.rendered_html * + table {
  margin-top: 1em;
}
.rendered_html p {
  text-align: left;
}
.rendered_html * + p {
  margin-top: 1em;
}
.rendered_html img {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.rendered_html * + img {
  margin-top: 1em;
}
.rendered_html img,
.rendered_html svg {
  max-width: 100%;
  height: auto;
}
.rendered_html img.unconfined,
.rendered_html svg.unconfined {
  max-width: none;
}
div.text_cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.text_cell > div.prompt {
    display: none;
  }
}
div.text_cell_render {
  /*font-family: "Helvetica Neue", Arial, Helvetica, Geneva, sans-serif;*/
  outline: none;
  resize: none;
  width: inherit;
  border-style: none;
  padding: 0.5em 0.5em 0.5em 0.4em;
  color: #000;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
a.anchor-link:link {
  text-decoration: none;
  padding: 0px 20px;
  visibility: hidden;
}
h1:hover .anchor-link,
h2:hover .anchor-link,
h3:hover .anchor-link,
h4:hover .anchor-link,
h5:hover .anchor-link,
h6:hover .anchor-link {
  visibility: visible;
}
.text_cell.rendered .input_area {
  display: none;
}
.text_cell.rendered .rendered_html {
  overflow-x: auto;
  overflow-y: hidden;
}
.text_cell.unrendered .text_cell_render {
  display: none;
}
.cm-header-1,
.cm-header-2,
.cm-header-3,
.cm-header-4,
.cm-header-5,
.cm-header-6 {
  font-weight: bold;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}
.cm-header-1 {
  font-size: 185.7%;
}
.cm-header-2 {
  font-size: 157.1%;
}
.cm-header-3 {
  font-size: 128.6%;
}
.cm-header-4 {
  font-size: 110%;
}
.cm-header-5 {
  font-size: 100%;
  font-style: italic;
}
.cm-header-6 {
  font-size: 100%;
  font-style: italic;
}
/*!
*
* IPython notebook webapp
*
*/
@media (max-width: 767px) {
  .notebook_app {
    padding-left: 0px;
    padding-right: 0px;
  }
}
#ipython-main-app {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook_panel {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook {
  font-size: 14px;
  line-height: 20px;
  overflow-y: hidden;
  overflow-x: auto;
  width: 100%;
  /* This spaces the page away from the edge of the notebook area */
  padding-top: 20px;
  margin: 0px;
  outline: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  min-height: 100%;
}
@media not print {
  #notebook-container {
    padding: 15px;
    background-color: #fff;
    min-height: 0;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
@media print {
  #notebook-container {
    width: 100%;
  }
}
div.ui-widget-content {
  border: 1px solid #ababab;
  outline: none;
}
pre.dialog {
  background-color: #f7f7f7;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0.4em;
  padding-left: 2em;
}
p.dialog {
  padding: 0.2em;
}
/* Word-wrap output correctly.  This is the CSS3 spelling, though Firefox seems
   to not honor it correctly.  Webkit browsers (Chrome, rekonq, Safari) do.
 */
pre,
code,
kbd,
samp {
  white-space: pre-wrap;
}
#fonttest {
  font-family: monospace;
}
p {
  margin-bottom: 0;
}
.end_space {
  min-height: 100px;
  transition: height .2s ease;
}
.notebook_app > #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
@media not print {
  .notebook_app {
    background-color: #EEE;
  }
}
kbd {
  border-style: solid;
  border-width: 1px;
  box-shadow: none;
  margin: 2px;
  padding-left: 2px;
  padding-right: 2px;
  padding-top: 1px;
  padding-bottom: 1px;
}
/* CSS for the cell toolbar */
.celltoolbar {
  border: thin solid #CFCFCF;
  border-bottom: none;
  background: #EEE;
  border-radius: 2px 2px 0px 0px;
  width: 100%;
  height: 29px;
  padding-right: 4px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
  display: -webkit-flex;
}
@media print {
  .celltoolbar {
    display: none;
  }
}
.ctb_hideshow {
  display: none;
  vertical-align: bottom;
}
/* ctb_show is added to the ctb_hideshow div to show the cell toolbar.
   Cell toolbars are only shown when the ctb_global_show class is also set.
*/
.ctb_global_show .ctb_show.ctb_hideshow {
  display: block;
}
.ctb_global_show .ctb_show + .input_area,
.ctb_global_show .ctb_show + div.text_cell_input,
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border-top-right-radius: 0px;
  border-top-left-radius: 0px;
}
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border: 1px solid #cfcfcf;
}
.celltoolbar {
  font-size: 87%;
  padding-top: 3px;
}
.celltoolbar select {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
  width: inherit;
  font-size: inherit;
  height: 22px;
  padding: 0px;
  display: inline-block;
}
.celltoolbar select:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.celltoolbar select::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.celltoolbar select:-ms-input-placeholder {
  color: #999;
}
.celltoolbar select::-webkit-input-placeholder {
  color: #999;
}
.celltoolbar select::-ms-expand {
  border: 0;
  background-color: transparent;
}
.celltoolbar select[disabled],
.celltoolbar select[readonly],
fieldset[disabled] .celltoolbar select {
  background-color: #eeeeee;
  opacity: 1;
}
.celltoolbar select[disabled],
fieldset[disabled] .celltoolbar select {
  cursor: not-allowed;
}
textarea.celltoolbar select {
  height: auto;
}
select.celltoolbar select {
  height: 30px;
  line-height: 30px;
}
textarea.celltoolbar select,
select[multiple].celltoolbar select {
  height: auto;
}
.celltoolbar label {
  margin-left: 5px;
  margin-right: 5px;
}
.completions {
  position: absolute;
  z-index: 110;
  overflow: hidden;
  border: 1px solid #ababab;
  border-radius: 2px;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  line-height: 1;
}
.completions select {
  background: white;
  outline: none;
  border: none;
  padding: 0px;
  margin: 0px;
  overflow: auto;
  font-family: monospace;
  font-size: 110%;
  color: #000;
  width: auto;
}
.completions select option.context {
  color: #286090;
}
#kernel_logo_widget {
  float: right !important;
  float: right;
}
#kernel_logo_widget .current_kernel_logo {
  display: none;
  margin-top: -1px;
  margin-bottom: -1px;
  width: 32px;
  height: 32px;
}
#menubar {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  margin-top: 1px;
}
#menubar .navbar {
  border-top: 1px;
  border-radius: 0px 0px 2px 2px;
  margin-bottom: 0px;
}
#menubar .navbar-toggle {
  float: left;
  padding-top: 7px;
  padding-bottom: 7px;
  border: none;
}
#menubar .navbar-collapse {
  clear: left;
}
.nav-wrapper {
  border-bottom: 1px solid #e7e7e7;
}
i.menu-icon {
  padding-top: 4px;
}
ul#help_menu li a {
  overflow: hidden;
  padding-right: 2.2em;
}
ul#help_menu li a i {
  margin-right: -1.2em;
}
.dropdown-submenu {
  position: relative;
}
.dropdown-submenu > .dropdown-menu {
  top: 0;
  left: 100%;
  margin-top: -6px;
  margin-left: -1px;
}
.dropdown-submenu:hover > .dropdown-menu {
  display: block;
}
.dropdown-submenu > a:after {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: block;
  content: "\f0da";
  float: right;
  color: #333333;
  margin-top: 2px;
  margin-right: -10px;
}
.dropdown-submenu > a:after.pull-left {
  margin-right: .3em;
}
.dropdown-submenu > a:after.pull-right {
  margin-left: .3em;
}
.dropdown-submenu:hover > a:after {
  color: #262626;
}
.dropdown-submenu.pull-left {
  float: none;
}
.dropdown-submenu.pull-left > .dropdown-menu {
  left: -100%;
  margin-left: 10px;
}
#notification_area {
  float: right !important;
  float: right;
  z-index: 10;
}
.indicator_area {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
#kernel_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  border-left: 1px solid;
}
#kernel_indicator .kernel_indicator_name {
  padding-left: 5px;
  padding-right: 5px;
}
#modal_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
#readonly-indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  margin-top: 2px;
  margin-bottom: 0px;
  margin-left: 0px;
  margin-right: 0px;
  display: none;
}
.modal_indicator:before {
  width: 1.28571429em;
  text-align: center;
}
.edit_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f040";
}
.edit_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.edit_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.command_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: ' ';
}
.command_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.command_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.kernel_idle_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f10c";
}
.kernel_idle_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_idle_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_busy_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f111";
}
.kernel_busy_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_busy_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_dead_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f1e2";
}
.kernel_dead_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_dead_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_disconnected_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f127";
}
.kernel_disconnected_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_disconnected_icon:before.pull-right {
  margin-left: .3em;
}
.notification_widget {
  color: #777;
  z-index: 10;
  background: rgba(240, 240, 240, 0.5);
  margin-right: 4px;
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget:focus,
.notification_widget.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.notification_widget:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active:hover,
.notification_widget.active:hover,
.open > .dropdown-toggle.notification_widget:hover,
.notification_widget:active:focus,
.notification_widget.active:focus,
.open > .dropdown-toggle.notification_widget:focus,
.notification_widget:active.focus,
.notification_widget.active.focus,
.open > .dropdown-toggle.notification_widget.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  background-image: none;
}
.notification_widget.disabled:hover,
.notification_widget[disabled]:hover,
fieldset[disabled] .notification_widget:hover,
.notification_widget.disabled:focus,
.notification_widget[disabled]:focus,
fieldset[disabled] .notification_widget:focus,
.notification_widget.disabled.focus,
.notification_widget[disabled].focus,
fieldset[disabled] .notification_widget.focus {
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget .badge {
  color: #fff;
  background-color: #333;
}
.notification_widget.warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning:focus,
.notification_widget.warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.notification_widget.warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active:hover,
.notification_widget.warning.active:hover,
.open > .dropdown-toggle.notification_widget.warning:hover,
.notification_widget.warning:active:focus,
.notification_widget.warning.active:focus,
.open > .dropdown-toggle.notification_widget.warning:focus,
.notification_widget.warning:active.focus,
.notification_widget.warning.active.focus,
.open > .dropdown-toggle.notification_widget.warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  background-image: none;
}
.notification_widget.warning.disabled:hover,
.notification_widget.warning[disabled]:hover,
fieldset[disabled] .notification_widget.warning:hover,
.notification_widget.warning.disabled:focus,
.notification_widget.warning[disabled]:focus,
fieldset[disabled] .notification_widget.warning:focus,
.notification_widget.warning.disabled.focus,
.notification_widget.warning[disabled].focus,
fieldset[disabled] .notification_widget.warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.notification_widget.success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success:focus,
.notification_widget.success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.notification_widget.success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active:hover,
.notification_widget.success.active:hover,
.open > .dropdown-toggle.notification_widget.success:hover,
.notification_widget.success:active:focus,
.notification_widget.success.active:focus,
.open > .dropdown-toggle.notification_widget.success:focus,
.notification_widget.success:active.focus,
.notification_widget.success.active.focus,
.open > .dropdown-toggle.notification_widget.success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  background-image: none;
}
.notification_widget.success.disabled:hover,
.notification_widget.success[disabled]:hover,
fieldset[disabled] .notification_widget.success:hover,
.notification_widget.success.disabled:focus,
.notification_widget.success[disabled]:focus,
fieldset[disabled] .notification_widget.success:focus,
.notification_widget.success.disabled.focus,
.notification_widget.success[disabled].focus,
fieldset[disabled] .notification_widget.success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.notification_widget.info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info:focus,
.notification_widget.info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.notification_widget.info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active:hover,
.notification_widget.info.active:hover,
.open > .dropdown-toggle.notification_widget.info:hover,
.notification_widget.info:active:focus,
.notification_widget.info.active:focus,
.open > .dropdown-toggle.notification_widget.info:focus,
.notification_widget.info:active.focus,
.notification_widget.info.active.focus,
.open > .dropdown-toggle.notification_widget.info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  background-image: none;
}
.notification_widget.info.disabled:hover,
.notification_widget.info[disabled]:hover,
fieldset[disabled] .notification_widget.info:hover,
.notification_widget.info.disabled:focus,
.notification_widget.info[disabled]:focus,
fieldset[disabled] .notification_widget.info:focus,
.notification_widget.info.disabled.focus,
.notification_widget.info[disabled].focus,
fieldset[disabled] .notification_widget.info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.notification_widget.danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger:focus,
.notification_widget.danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.notification_widget.danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active:hover,
.notification_widget.danger.active:hover,
.open > .dropdown-toggle.notification_widget.danger:hover,
.notification_widget.danger:active:focus,
.notification_widget.danger.active:focus,
.open > .dropdown-toggle.notification_widget.danger:focus,
.notification_widget.danger:active.focus,
.notification_widget.danger.active.focus,
.open > .dropdown-toggle.notification_widget.danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  background-image: none;
}
.notification_widget.danger.disabled:hover,
.notification_widget.danger[disabled]:hover,
fieldset[disabled] .notification_widget.danger:hover,
.notification_widget.danger.disabled:focus,
.notification_widget.danger[disabled]:focus,
fieldset[disabled] .notification_widget.danger:focus,
.notification_widget.danger.disabled.focus,
.notification_widget.danger[disabled].focus,
fieldset[disabled] .notification_widget.danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger .badge {
  color: #d9534f;
  background-color: #fff;
}
div#pager {
  background-color: #fff;
  font-size: 14px;
  line-height: 20px;
  overflow: hidden;
  display: none;
  position: fixed;
  bottom: 0px;
  width: 100%;
  max-height: 50%;
  padding-top: 8px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  /* Display over codemirror */
  z-index: 100;
  /* Hack which prevents jquery ui resizable from changing top. */
  top: auto !important;
}
div#pager pre {
  line-height: 1.21429em;
  color: #000;
  background-color: #f7f7f7;
  padding: 0.4em;
}
div#pager #pager-button-area {
  position: absolute;
  top: 8px;
  right: 20px;
}
div#pager #pager-contents {
  position: relative;
  overflow: auto;
  width: 100%;
  height: 100%;
}
div#pager #pager-contents #pager-container {
  position: relative;
  padding: 15px 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
div#pager .ui-resizable-handle {
  top: 0px;
  height: 8px;
  background: #f7f7f7;
  border-top: 1px solid #cfcfcf;
  border-bottom: 1px solid #cfcfcf;
  /* This injects handle bars (a short, wide = symbol) for 
        the resize handle. */
}
div#pager .ui-resizable-handle::after {
  content: '';
  top: 2px;
  left: 50%;
  height: 3px;
  width: 30px;
  margin-left: -15px;
  position: absolute;
  border-top: 1px solid #cfcfcf;
}
.quickhelp {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  line-height: 1.8em;
}
.shortcut_key {
  display: inline-block;
  width: 21ex;
  text-align: right;
  font-family: monospace;
}
.shortcut_descr {
  display: inline-block;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
span.save_widget {
  margin-top: 6px;
}
span.save_widget span.filename {
  height: 1em;
  line-height: 1em;
  padding: 3px;
  margin-left: 16px;
  border: none;
  font-size: 146.5%;
  border-radius: 2px;
}
span.save_widget span.filename:hover {
  background-color: #e6e6e6;
}
span.checkpoint_status,
span.autosave_status {
  font-size: small;
}
@media (max-width: 767px) {
  span.save_widget {
    font-size: small;
  }
  span.checkpoint_status,
  span.autosave_status {
    display: none;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  span.checkpoint_status {
    display: none;
  }
  span.autosave_status {
    font-size: x-small;
  }
}
.toolbar {
  padding: 0px;
  margin-left: -5px;
  margin-top: 2px;
  margin-bottom: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.toolbar select,
.toolbar label {
  width: auto;
  vertical-align: middle;
  margin-right: 2px;
  margin-bottom: 0px;
  display: inline;
  font-size: 92%;
  margin-left: 0.3em;
  margin-right: 0.3em;
  padding: 0px;
  padding-top: 3px;
}
.toolbar .btn {
  padding: 2px 8px;
}
.toolbar .btn-group {
  margin-top: 0px;
  margin-left: 5px;
}
#maintoolbar {
  margin-bottom: -3px;
  margin-top: -8px;
  border: 0px;
  min-height: 27px;
  margin-left: 0px;
  padding-top: 11px;
  padding-bottom: 3px;
}
#maintoolbar .navbar-text {
  float: none;
  vertical-align: middle;
  text-align: right;
  margin-left: 5px;
  margin-right: 0px;
  margin-top: 0px;
}
.select-xs {
  height: 24px;
}
.pulse,
.dropdown-menu > li > a.pulse,
li.pulse > a.dropdown-toggle,
li.pulse.open > a.dropdown-toggle {
  background-color: #F37626;
  color: white;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
/** WARNING IF YOU ARE EDITTING THIS FILE, if this is a .css file, It has a lot
 * of chance of beeing generated from the ../less/[samename].less file, you can
 * try to get back the less file by reverting somme commit in history
 **/
/*
 * We'll try to get something pretty, so we
 * have some strange css to have the scroll bar on
 * the left with fix button on the top right of the tooltip
 */
@-moz-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-webkit-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-moz-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@-webkit-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
/*properties of tooltip after "expand"*/
.bigtooltip {
  overflow: auto;
  height: 200px;
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
}
/*properties of tooltip before "expand"*/
.smalltooltip {
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
  text-overflow: ellipsis;
  overflow: hidden;
  height: 80px;
}
.tooltipbuttons {
  position: absolute;
  padding-right: 15px;
  top: 0px;
  right: 0px;
}
.tooltiptext {
  /*avoid the button to overlap on some docstring*/
  padding-right: 30px;
}
.ipython_tooltip {
  max-width: 700px;
  /*fade-in animation when inserted*/
  -webkit-animation: fadeOut 400ms;
  -moz-animation: fadeOut 400ms;
  animation: fadeOut 400ms;
  -webkit-animation: fadeIn 400ms;
  -moz-animation: fadeIn 400ms;
  animation: fadeIn 400ms;
  vertical-align: middle;
  background-color: #f7f7f7;
  overflow: visible;
  border: #ababab 1px solid;
  outline: none;
  padding: 3px;
  margin: 0px;
  padding-left: 7px;
  font-family: monospace;
  min-height: 50px;
  -moz-box-shadow: 0px 6px 10px -1px #adadad;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  border-radius: 2px;
  position: absolute;
  z-index: 1000;
}
.ipython_tooltip a {
  float: right;
}
.ipython_tooltip .tooltiptext pre {
  border: 0;
  border-radius: 0;
  font-size: 100%;
  background-color: #f7f7f7;
}
.pretooltiparrow {
  left: 0px;
  margin: 0px;
  top: -16px;
  width: 40px;
  height: 16px;
  overflow: hidden;
  position: absolute;
}
.pretooltiparrow:before {
  background-color: #f7f7f7;
  border: 1px #ababab solid;
  z-index: 11;
  content: "";
  position: absolute;
  left: 15px;
  top: 10px;
  width: 25px;
  height: 25px;
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
}
ul.typeahead-list i {
  margin-left: -10px;
  width: 18px;
}
ul.typeahead-list {
  max-height: 80vh;
  overflow: auto;
}
ul.typeahead-list > li > a {
  /** Firefox bug **/
  /* see https://github.com/jupyter/notebook/issues/559 */
  white-space: normal;
}
.cmd-palette .modal-body {
  padding: 7px;
}
.cmd-palette form {
  background: white;
}
.cmd-palette input {
  outline: none;
}
.no-shortcut {
  display: none;
}
.command-shortcut:before {
  content: "(command)";
  padding-right: 3px;
  color: #777777;
}
.edit-shortcut:before {
  content: "(edit)";
  padding-right: 3px;
  color: #777777;
}
#find-and-replace #replace-preview .match,
#find-and-replace #replace-preview .insert {
  background-color: #BBDEFB;
  border-color: #90CAF9;
  border-style: solid;
  border-width: 1px;
  border-radius: 0px;
}
#find-and-replace #replace-preview .replace .match {
  background-color: #FFCDD2;
  border-color: #EF9A9A;
  border-radius: 0px;
}
#find-and-replace #replace-preview .replace .insert {
  background-color: #C8E6C9;
  border-color: #A5D6A7;
  border-radius: 0px;
}
#find-and-replace #replace-preview {
  max-height: 60vh;
  overflow: auto;
}
#find-and-replace #replace-preview pre {
  padding: 5px 10px;
}
.terminal-app {
  background: #EEE;
}
.terminal-app #header {
  background: #fff;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.terminal-app .terminal {
  width: 100%;
  float: left;
  font-family: monospace;
  color: white;
  background: black;
  padding: 0.4em;
  border-radius: 2px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
}
.terminal-app .terminal,
.terminal-app .terminal dummy-screen {
  line-height: 1em;
  font-size: 14px;
}
.terminal-app .terminal .xterm-rows {
  padding: 10px;
}
.terminal-app .terminal-cursor {
  color: black;
  background: white;
}
.terminal-app #terminado-container {
  margin-top: 20px;
}
/*# sourceMappingURL=style.min.css.map */
    </style>
<style type="text/css">
    .highlight .hll { background-color: #ffffcc }
.highlight  { background: #f8f8f8; }
.highlight .c { color: #408080; font-style: italic } /* Comment */
.highlight .err { border: 1px solid #FF0000 } /* Error */
.highlight .k { color: #008000; font-weight: bold } /* Keyword */
.highlight .o { color: #666666 } /* Operator */
.highlight .ch { color: #408080; font-style: italic } /* Comment.Hashbang */
.highlight .cm { color: #408080; font-style: italic } /* Comment.Multiline */
.highlight .cp { color: #BC7A00 } /* Comment.Preproc */
.highlight .cpf { color: #408080; font-style: italic } /* Comment.PreprocFile */
.highlight .c1 { color: #408080; font-style: italic } /* Comment.Single */
.highlight .cs { color: #408080; font-style: italic } /* Comment.Special */
.highlight .gd { color: #A00000 } /* Generic.Deleted */
.highlight .ge { font-style: italic } /* Generic.Emph */
.highlight .gr { color: #FF0000 } /* Generic.Error */
.highlight .gh { color: #000080; font-weight: bold } /* Generic.Heading */
.highlight .gi { color: #00A000 } /* Generic.Inserted */
.highlight .go { color: #888888 } /* Generic.Output */
.highlight .gp { color: #000080; font-weight: bold } /* Generic.Prompt */
.highlight .gs { font-weight: bold } /* Generic.Strong */
.highlight .gu { color: #800080; font-weight: bold } /* Generic.Subheading */
.highlight .gt { color: #0044DD } /* Generic.Traceback */
.highlight .kc { color: #008000; font-weight: bold } /* Keyword.Constant */
.highlight .kd { color: #008000; font-weight: bold } /* Keyword.Declaration */
.highlight .kn { color: #008000; font-weight: bold } /* Keyword.Namespace */
.highlight .kp { color: #008000 } /* Keyword.Pseudo */
.highlight .kr { color: #008000; font-weight: bold } /* Keyword.Reserved */
.highlight .kt { color: #B00040 } /* Keyword.Type */
.highlight .m { color: #666666 } /* Literal.Number */
.highlight .s { color: #BA2121 } /* Literal.String */
.highlight .na { color: #7D9029 } /* Name.Attribute */
.highlight .nb { color: #008000 } /* Name.Builtin */
.highlight .nc { color: #0000FF; font-weight: bold } /* Name.Class */
.highlight .no { color: #880000 } /* Name.Constant */
.highlight .nd { color: #AA22FF } /* Name.Decorator */
.highlight .ni { color: #999999; font-weight: bold } /* Name.Entity */
.highlight .ne { color: #D2413A; font-weight: bold } /* Name.Exception */
.highlight .nf { color: #0000FF } /* Name.Function */
.highlight .nl { color: #A0A000 } /* Name.Label */
.highlight .nn { color: #0000FF; font-weight: bold } /* Name.Namespace */
.highlight .nt { color: #008000; font-weight: bold } /* Name.Tag */
.highlight .nv { color: #19177C } /* Name.Variable */
.highlight .ow { color: #AA22FF; font-weight: bold } /* Operator.Word */
.highlight .w { color: #bbbbbb } /* Text.Whitespace */
.highlight .mb { color: #666666 } /* Literal.Number.Bin */
.highlight .mf { color: #666666 } /* Literal.Number.Float */
.highlight .mh { color: #666666 } /* Literal.Number.Hex */
.highlight .mi { color: #666666 } /* Literal.Number.Integer */
.highlight .mo { color: #666666 } /* Literal.Number.Oct */
.highlight .sa { color: #BA2121 } /* Literal.String.Affix */
.highlight .sb { color: #BA2121 } /* Literal.String.Backtick */
.highlight .sc { color: #BA2121 } /* Literal.String.Char */
.highlight .dl { color: #BA2121 } /* Literal.String.Delimiter */
.highlight .sd { color: #BA2121; font-style: italic } /* Literal.String.Doc */
.highlight .s2 { color: #BA2121 } /* Literal.String.Double */
.highlight .se { color: #BB6622; font-weight: bold } /* Literal.String.Escape */
.highlight .sh { color: #BA2121 } /* Literal.String.Heredoc */
.highlight .si { color: #BB6688; font-weight: bold } /* Literal.String.Interpol */
.highlight .sx { color: #008000 } /* Literal.String.Other */
.highlight .sr { color: #BB6688 } /* Literal.String.Regex */
.highlight .s1 { color: #BA2121 } /* Literal.String.Single */
.highlight .ss { color: #19177C } /* Literal.String.Symbol */
.highlight .bp { color: #008000 } /* Name.Builtin.Pseudo */
.highlight .fm { color: #0000FF } /* Name.Function.Magic */
.highlight .vc { color: #19177C } /* Name.Variable.Class */
.highlight .vg { color: #19177C } /* Name.Variable.Global */
.highlight .vi { color: #19177C } /* Name.Variable.Instance */
.highlight .vm { color: #19177C } /* Name.Variable.Magic */
.highlight .il { color: #666666 } /* Literal.Number.Integer.Long */
    </style>
<style type="text/css">
    
/* Temporary definitions which will become obsolete with Notebook release 5.0 */
.ansi-black-fg { color: #3E424D; }
.ansi-black-bg { background-color: #3E424D; }
.ansi-black-intense-fg { color: #282C36; }
.ansi-black-intense-bg { background-color: #282C36; }
.ansi-red-fg { color: #E75C58; }
.ansi-red-bg { background-color: #E75C58; }
.ansi-red-intense-fg { color: #B22B31; }
.ansi-red-intense-bg { background-color: #B22B31; }
.ansi-green-fg { color: #00A250; }
.ansi-green-bg { background-color: #00A250; }
.ansi-green-intense-fg { color: #007427; }
.ansi-green-intense-bg { background-color: #007427; }
.ansi-yellow-fg { color: #DDB62B; }
.ansi-yellow-bg { background-color: #DDB62B; }
.ansi-yellow-intense-fg { color: #B27D12; }
.ansi-yellow-intense-bg { background-color: #B27D12; }
.ansi-blue-fg { color: #208FFB; }
.ansi-blue-bg { background-color: #208FFB; }
.ansi-blue-intense-fg { color: #0065CA; }
.ansi-blue-intense-bg { background-color: #0065CA; }
.ansi-magenta-fg { color: #D160C4; }
.ansi-magenta-bg { background-color: #D160C4; }
.ansi-magenta-intense-fg { color: #A03196; }
.ansi-magenta-intense-bg { background-color: #A03196; }
.ansi-cyan-fg { color: #60C6C8; }
.ansi-cyan-bg { background-color: #60C6C8; }
.ansi-cyan-intense-fg { color: #258F8F; }
.ansi-cyan-intense-bg { background-color: #258F8F; }
.ansi-white-fg { color: #C5C1B4; }
.ansi-white-bg { background-color: #C5C1B4; }
.ansi-white-intense-fg { color: #A1A6B2; }
.ansi-white-intense-bg { background-color: #A1A6B2; }

.ansi-bold { font-weight: bold; }

    </style>


<style type="text/css">
/* Overrides of notebook CSS for static HTML export */
body {
  overflow: visible;
  padding: 8px;
}

div#notebook {
  overflow: visible;
  border-top: none;
}@media print {
  div.cell {
    display: block;
    page-break-inside: avoid;
  } 
  div.output_wrapper { 
    display: block;
    page-break-inside: avoid; 
  }
  div.output { 
    display: block;
    page-break-inside: avoid; 
  }
}
</style>

<!-- Custom stylesheet, it must be in the same directory as the html file -->
<link rel="stylesheet" href="custom.css">

<!-- Loading mathjax macro -->
<!-- Load mathjax -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS_HTML"></script>
    <!-- MathJax configuration -->
    <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        tex2jax: {
            inlineMath: [ ['$','$'], ["\\(","\\)"] ],
            displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
            processEscapes: true,
            processEnvironments: true
        },
        // Center justify equations in code and markdown cells. Elsewhere
        // we use CSS to left justify single line equations in code cells.
        displayAlign: 'center',
        "HTML-CSS": {
            styles: {'.MathJax_Display': {"margin": 0}},
            linebreaks: { automatic: true }
        }
    });
    </script>
    <!-- End of mathjax configuration --></head>
<body>
  <div tabindex="-1" id="notebook" class="border-box-sizing">

<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">


</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[118]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">matplotlib</span> inline 
<span class="kn">from</span> <span class="nn">sklearn.neighbors.kde</span> <span class="k">import</span> <span class="n">KernelDensity</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[119]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">([[</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">],</span> <span class="p">[</span><span class="o">-</span><span class="mi">2</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">],</span> <span class="p">[</span><span class="o">-</span><span class="mi">3</span><span class="p">,</span> <span class="o">-</span><span class="mi">2</span><span class="p">],</span> <span class="p">[</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">],</span> <span class="p">[</span><span class="mi">2</span><span class="p">,</span> <span class="mi">1</span><span class="p">],</span> <span class="p">[</span><span class="mi">3</span><span class="p">,</span> <span class="mi">2</span><span class="p">]])</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[120]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">X</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt output_prompt">Out[120]:</div>




<div class="output_text output_subarea output_execute_result">
<pre>array([[-1, -1],
       [-2, -1],
       [-3, -2],
       [ 1,  1],
       [ 2,  1],
       [ 3,  2]])</pre>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>when kernel is gaussian</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>$score = exp(-\frac{x^2}{2h^2})$</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>$h : width$</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>$x$: eachpoint in the group</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[121]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># model</span>
<span class="n">kde</span> <span class="o">=</span> <span class="n">KernelDensity</span><span class="p">(</span><span class="n">kernel</span><span class="o">=</span><span class="s1">&#39;gaussian&#39;</span><span class="p">,</span> <span class="n">bandwidth</span><span class="o">=</span><span class="mf">0.2</span><span class="p">)</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X</span><span class="p">)</span>

<span class="c1"># score</span>
<span class="n">kde</span><span class="o">.</span><span class="n">score_samples</span><span class="p">(</span><span class="n">X</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt output_prompt">Out[121]:</div>




<div class="output_text output_subarea output_execute_result">
<pre>array([-0.41075698, -0.41075698, -0.41076071, -0.41075698, -0.41075698,
       -0.41076071])</pre>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[132]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># due to be a distribution </span>
<span class="n">np</span><span class="o">.</span><span class="n">exp</span><span class="p">(</span><span class="n">kde</span><span class="o">.</span><span class="n">score_samples</span><span class="p">(</span><span class="n">X</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt output_prompt">Out[132]:</div>




<div class="output_text output_subarea output_execute_result">
<pre>array([ 0.04948882,  0.0812796 ,  0.08024707,  0.07697542,  0.05262884,
        0.03727848,  0.08788689,  0.20062386,  0.20236411,  0.22750219,
        0.10957551,  0.0655415 ,  0.22675675,  0.22529147,  0.13442135,
        0.16217474,  0.22714558,  0.18851256,  0.22046072,  0.18014493])</pre>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[122]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="kn">from</span> <span class="nn">scipy.stats</span> <span class="k">import</span> <span class="n">norm</span>
<span class="kn">from</span> <span class="nn">sklearn.neighbors</span> <span class="k">import</span> <span class="n">KernelDensity</span>


<span class="c1">#----------------------------------------------------------------------</span>
<span class="c1"># Plot the progression of histograms to kernels</span>
<span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">seed</span><span class="p">(</span><span class="mi">1</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[123]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">N</span> <span class="o">=</span> <span class="mi">20</span>
<span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">((</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">normal</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="nb">int</span><span class="p">(</span><span class="mf">0.3</span> <span class="o">*</span> <span class="n">N</span><span class="p">)),</span>
                    <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">normal</span><span class="p">(</span><span class="mi">5</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="nb">int</span><span class="p">(</span><span class="mf">0.7</span> <span class="o">*</span> <span class="n">N</span><span class="p">))))[:,</span> <span class="n">np</span><span class="o">.</span><span class="n">newaxis</span><span class="p">]</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[124]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">X</span><span class="o">.</span><span class="n">shape</span> <span class="c1"># data point</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt output_prompt">Out[124]:</div>




<div class="output_text output_subarea output_execute_result">
<pre>(20, 1)</pre>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[133]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># linspace: split the line by n</span>
<span class="c1"># np.newaxis: makes nth axis </span>
<span class="c1"># : : all row</span>
<span class="n">X_plot</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="o">-</span><span class="mi">5</span><span class="p">,</span> <span class="mi">10</span><span class="p">,</span> <span class="mi">1000</span><span class="p">)[:,</span> <span class="n">np</span><span class="o">.</span><span class="n">newaxis</span><span class="p">]</span> <span class="c1"># (1000,)-&gt; (1000,1)</span>
<span class="n">bins</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="o">-</span><span class="mi">5</span><span class="p">,</span> <span class="mi">10</span><span class="p">,</span> <span class="mi">10</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[134]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">fig</span><span class="p">,</span> <span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">subplots</span><span class="p">(</span><span class="mi">2</span><span class="p">,</span> <span class="mi">2</span><span class="p">,</span> <span class="n">sharex</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">sharey</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
rkJggg==
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[137]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">fig</span><span class="p">,</span> <span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">subplots</span><span class="p">(</span><span class="mi">2</span><span class="p">,</span> <span class="mi">2</span><span class="p">,</span> <span class="n">sharex</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">sharey</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="c1"># reduce interval of grids</span>
<span class="n">fig</span><span class="o">.</span><span class="n">subplots_adjust</span><span class="p">(</span><span class="n">hspace</span><span class="o">=</span><span class="mf">0.05</span><span class="p">,</span> <span class="n">wspace</span><span class="o">=</span><span class="mf">0.05</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAXwAAAD8CAYAAAB0IB+mAAAABHNCSVQICAgIfAhkiAAAAAlwSFlz
AAALEgAACxIB0t1+/AAAEX5JREFUeJzt3W+spGV5x/HvTxBNEbFx18TwRzAu4paagCdIQ1JpxGah
CbywMWxCLGbDRqvGBGqi0ajBV2qriQlW1pT6JxFFX5iTuhRThZgQF1mKoqzRroi6aAsi8obIn/Tq
i3mox+EczsOZ+5wze+7vJ9lk5pl7574ye+3vzJlnZq5UFZKkre85m12AJGljGPiS1AkDX5I6YeBL
UicMfEnqhIEvSZ1YNfCTXJ/kgSQ/XOH2JPlkksNJ7k5yTvsyJUmzGvMM/7PArme4/SJgx/BnL/DP
s5clSWpt1cCvqm8Dv32GJZcCn6+JA8CLkry0VYGSpDaObXAfJwG/XHL9yHDs19MLk+xl8lsAxx9/
/GvOPPPMBttLs7nzzjt/U1Xbp4/br5pHK/XrGC0Cf7Sq2gfsA1hYWKiDBw9u5PbSspL8fLnj9qvm
0Ur9OkaLd+ncD5yy5PrJwzFJ0hxpEfiLwJuHd+ucBzxSVU97OUeStLlWfUknyQ3ABcC2JEeADwLP
BaiqTwP7gYuBw8CjwFvWq1hJ0tqtGvhVtXuV2wt4e7OKJEnrwk/aSlInDHxJ6oSBL0mdMPAlqRMG
viR1wsCXpE4Y+JLUCQNfkjph4EtSJwx8SeqEgS9JnTDwJakTowI/ya4kPx4Glb9nmdtPTXJLkruG
QeYXty9VkjSLVQM/yTHAtUyGle8EdifZObXs/cCNVXU2cBnwqdaFSpJmM+YZ/rnA4aq6t6oeB77E
ZHD5UgW8cLh8IvCrdiVKkloYM9N2uSHlr51a8yHgG0neCRwPXNikOklSM61O2u4GPltVJzOZfvWF
JE+77yR7kxxMcvDBBx9stLW0PuxXbTVjAn/MkPI9wI0AVfUd4PnAtuk7qqp9VbVQVQvbt29fW8XS
BrFftdWMCfw7gB1JTk9yHJOTsotTa34BvB4gyauYBL5PiSRpjqwa+FX1JPAO4GbgR0zejXNPkmuS
XDIsuxq4Msn3gRuAK4ZZt5KkOTHmpC1VtR/YP3XsA0suHwLOb1uaJKklP2krSZ0w8CWpEwa+JHXC
wJekThj4ktQJA1+SOmHgS1InDHxJ6oSBL0mdMPAlqRMGviR1wsCXpE4Y+JLUiVGBn2RXkh8nOZzk
PSuseVOSQ0nuSfLFtmVKkma16tcjJzkGuBZ4A5N5tnckWRy+EvmpNTuA9wLnV9XDSV6yXgVLktZm
zDP8c4HDVXVvVT0OfAm4dGrNlcC1VfUwQFU90LZMSdKsxgT+ScAvl1w/Mhxb6gzgjCS3JTmQZNdy
d+RQaB1N7FdtNa1O2h4L7AAuAHYDn0nyoulFDoXW0cR+1VYzJvDvB05Zcv3k4dhSR4DFqnqiqn4G
/ITJDwBJ0pwYE/h3ADuSnJ7kOOAyYHFqzdeYPLsnyTYmL/Hc27BOSdKMVg38qnoSeAdwM/Aj4Maq
uifJNUkuGZbdDDyU5BBwC/DuqnpovYqWJD17q74tE6Cq9gP7p459YMnlAq4a/kiS5pCftJWkThj4
ktQJA1+SOmHgS1InDHxJ6oSBL0mdMPAlqRMGviR1wsCXpE4Y+JLUCQNfkjph4EtSJ5oNMR/WvTFJ
JVloV6IkqYVVA3/JEPOLgJ3A7iQ7l1l3AvAu4PbWRUqSZtdqiDnAh4GPAL9vWJ8kqZEmQ8yTnAOc
UlVff6Y7cii0jib2q7aamU/aJnkO8HHg6tXWOhRaRxP7VVtNiyHmJwBnAbcmuQ84D1j0xK0kzZeZ
h5hX1SNVta2qTquq04ADwCVVdXBdKpYkrUmrIeaSpDnXZIj51PELZi9LktSan7SVpE4Y+JLUCQNf
kjph4EtSJwx8SeqEgS9JnTDwJakTBr4kdcLAl6ROGPiS1AkDX5I6YeBLUieaDDFPclWSQ0nuTvLN
JC9rX6okaRathpjfBSxU1auBrwIfbV2oJGk2TYaYV9UtVfXocPUAk6lYkqQ50mSI+ZQ9wE3L3eBQ
aB1N7FdtNU1P2ia5HFgAPrbc7Q6F1tHEftVWM2bi1WpDzAFIciHwPuB1VfVYm/IkSa3MPMQcIMnZ
wHVMhpc/0L5MSdKsWg0x/xjwAuArSb6XZHGFu5MkbZImQ8yr6sLGdUmSGvOTtpLUCQNfkjph4EtS
Jwx8SeqEgS9JnTDwJakTBr4kdcLAl6ROGPiS1AkDX5I6YeBLUicMfEnqRKsh5s9L8uXh9tuTnNa6
UEnSbFoNMd8DPFxVrwA+AXykdaGSpNk0GWI+XP/ccPmrwOuTpF2ZkqRZjfk+/OWGmL92pTVV9WSS
R4AXA79ZuijJXmDvcPWxJD9cS9ENbWOqxs72t4aJVy53cM76dbMfI2uYj/1hhX4dY9QAlFaqah+w
DyDJwapa2Mj9p212DZu9vzX8Yf/ljs9Tv272/tYwH/s/VcNa/+6Yl3TGDDH//zVJjgVOBB5aa1GS
pPbGBP5e4MLhXTpPG2I+vFZ/HPDvSe4G/gH4VlXVehQsSVqbMS/p/Cvwn8A/Mhlifv1TQ8yBg8CT
wOPAN4DzgPcDrx5xv/vWVHFbm13DZu8P1jB2/6OhxvVmDZu/P8xQQ8Y8ER/eV/9vVXXWMrddB9xa
VTcM138MXFBVv15rUZKk9lqctF3uXTwnAU8L/KXvejj++ONfc+aZZzbYXprNnXfe+Zuq2j593H7V
PFqpX8fYtHfpLCws1MGDaz7ZLDWT5OfLHbdfNY9W6tcxWnyXzph38UiSNlmLwF8E3pyJ84BHfP1e
kubPqi/pJLkBuADYluQI8EHguQBV9WlgP3AxcBh4FHjLehUrSVq7VQO/qnavcnsBb29WkSRpXfh9
+JLUCQNfkjph4EtSJwx8SeqEgS9JnTDwJakTBr4kdcLAl6ROGPiS1AkDX5I6YeBLUicMfEnqxKjA
T7JrGGJ+OMl7lrn91CS3JLkryd1JLm5fqiRpFqsGfpJjgGuBi4CdwO4kO6eWvR+4sarOBi4DPtW6
UEnSbMY8wz8XOFxV91bV48CXgEun1hTwwuHyicCv2pUoSWphTOCvNKR8qQ8Blw8DUvYD71zujpLs
TXIwycEHH3xwDeVKG8d+1VbT6qTtbuCzVXUyk+lXX0jytPuuqn1VtVBVC9u3r2nourRh7FdtNWMC
f8yQ8j3AjQBV9R3g+cC2FgVKktoYE/h3ADuSnJ7kOCYnZRen1vwCeD1AklcxCXx/B5akObJq4FfV
k8A7gJuBHzF5N849Sa5Jcsmw7GrgyiTfB24Arhhm3UqS5sSqQ8wBqmo/k5OxS499YMnlQ8D5bUuT
JLXkJ20lqRMGviR1wsCXpE4Y+JLUCQNfkjph4EtSJwx8SeqEgS9JnTDwJakTBr4kdcLAl6ROGPiS
1IkmQ8yHNW9KcijJPUm+2LZMSdKsVv22zCVDzN/AZLzhHUkWh2/IfGrNDuC9wPlV9XCSl6xXwZKk
tWk1xPxK4Nqqehigqh5oW6YkaVathpifAZyR5LYkB5LsWu6OHAqto4n9qq2m1UnbY4EdwAVMBpp/
JsmLphc5FFpHE/tVW02rIeZHgMWqeqKqfgb8hMkPAEnSnGg1xPxrTJ7dk2Qbk5d47m1YpyRpRq2G
mN8MPJTkEHAL8O6qemi9ipYkPXuthpgXcNXwR5I0h/ykrSR1wsCXpE4Y+JLUCQNfkjph4EtSJwx8
SeqEgS9JnTDwJakTBr4kdcLAl6ROGPiS1AkDX5I60WyI+bDujUkqyUK7EiVJLawa+EuGmF8E7AR2
J9m5zLoTgHcBt7cuUpI0u1ZDzAE+DHwE+H3D+iRJjTQZYp7kHOCUqvr6M92RQ6F1NLFftdXMfNI2
yXOAjwNXr7bWodA6mtiv2mpaDDE/ATgLuDXJfcB5wKInbiVpvsw8xLyqHqmqbVV1WlWdBhwALqmq
g+tSsSRpTVoNMZckzbkmQ8ynjl8we1mSpNb8pK0kdcLAl6ROGPiS1AkDX5I6YeBLUicMfEnqhIEv
SZ0w8CWpEwa+JHXCwJekThj4ktQJA1+SOtFkiHmSq5IcSnJ3km8meVn7UiVJs2g1xPwuYKGqXg18
Ffho60IlSbNpMsS8qm6pqkeHqweYTMWSJM2RJkPMp+wBbpqlKElSe6MGoIyV5HJgAXjdCrfvBfYC
nHrqqS23lpqzX7XVtBhiDkCSC4H3MZln+9hyd1RV+6pqoaoWtm/fvpZ6pQ1jv2qrmXmIOUCSs4Hr
mIT9A+3LlCTNqtUQ848BLwC+kuR7SRZXuDtJ0iZpMsS8qi5sXJckqTE/aStJnTDwJakTBr4kdcLA
l6ROGPiS1AkDX5I6YeBLUicMfEnqhIEvSZ0w8CWpEwa+JHXCwJekThj4ktSJUYGfZFeSHyc5nOQ9
y9z+vCRfHm6/PclprQuVJM1m1cBPcgxwLXARsBPYnWTn1LI9wMNV9QrgE8BHWhcqSZrNmGf45wKH
q+reqnoc+BJw6dSaS4HPDZe/Crw+SdqVKUma1ZgBKCcBv1xy/Qjw2pXWVNWTSR4BXgz8ZumipUOh
gceS/HAtRTe0jakaO9vfGiZeudzBOevXzX6MrGE+9ocV+nWMUROvWqmqfcA+gCQHq2phI/efttk1
bPb+1vCH/Zc7Pk/9utn7W8N87P9UDWv9u2Ne0rkfOGXJ9ZOHY8uuSXIscCLw0FqLkiS1Nybw7wB2
JDk9yXHAZcD0kPJF4O+Gy38LfKuqql2ZkqRZrfqSzvCa/DuAm4FjgOur6p4k1wAHq2oR+BfgC0kO
A79l8kNhNftmqLuVza5hs/cHaxi7/9FQ43qzhs3fH2aoIT4Rl6Q++ElbSeqEgS9JnVj3wN/sr2UY
sf9VSQ4luTvJN5O8rOX+Y2pYsu6NSSpJ87d9jakhyZuGx+KeJF/cyP2TnJrkliR3Df8WF7fcf9jj
+iQPrPR++qHGh5M8nuTXSc6Zun3dv0LEft38Xh1Tw3r364heTZJPDvXdPd2rK6qqdfvD5CTvT4GX
A8cB3wd2Tq35e+DTw+XLgC9v8P5/BfzJcPltLfcfW8Ow7gTg28ABYGET/h12AHcBfzpcf8kG778P
eNtweSdw3zr0418C5wA/XKHGXwO3DjX+F3D3RvWq/TofvTov/fpMvTrcfjFwExDgPOD2Mfe73s/w
N/trGVbdv6puqapHh6sHmHzOoKUxjwHAh5l8B9HvG+8/toYrgWur6mGAqnpgg/cv4IXD5ROBXzXc
f7JB1beZvItspRqfBK4barweeGmSly5Zs95fIWK/bn6vjq1hXft1lV5lqOfzNXEAeNFUry5rvQN/
ua9lOGmlNVX1JPDU1zJs1P5L7WHyU7OlVWsYfh07paq+3njv0TUAZwBnJLktyYEkuzZ4/w8Blyc5
AuwH3tlw/zFOYvKf+Kk6jzAJs5Om1qxXr/7R/S+pobd+3exeHVvDh9j8fn02vQJs8FcrzLMklwML
wOs2eN/nAB8HrtjIfZdxLJNflS9g8qzx20n+vKp+t0H77wY+W1X/lOQvmHyu46yq+t8N2v+o0nm/
bnavwlHar+v9DH+zv5ZhzP4kuRB4H3BJVT3WaO+xNZwAnAXcmuQ+Jq/HLTY+ETbmcTgCLFbVE1X1
M+AnTP5TbdT+e4AbAarqO8DzmXxR1Ua5n8nroU/VefJQw/1Ta9bzK0Ts183v1bE1zEO/rtorT9Py
RMMyJxaOBe4FTucPJz/+bGrN2/njE2E3bvD+ZzM5QbNjsx6DqfW30v6k7ZjHYRfwueHyNia/Lr54
A/e/CbhiuPwqJq+JZh3+PU5j+ZO2x/L0k7Y/2KhetV/no1fnqV9X6tXhtr/hj0/afnfUfa5H00wV
djGTn8A/Bd43HLuGybMTmPxk/ApwGPgu8PIN3v8/gP8Bvjf8Wdzox2BqbdP/QM/icQiTX9UPAT8A
Ltvg/XcCtw3/ub4H/PU6PAY3MAn1J5g8S9wDvBV465Iafzfc/t9MXjLZsF61X+ejV+ehX0f0apgM
pvrp8BiM+jfwqxUkqRN+0laSOmHgS1InDHxJ6oSBL0mdMPAlqRMGviR1wsCXpE78HwNJeC4505Uf
AAAAAElFTkSuQmCC
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[138]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">(</span><span class="n">X</span><span class="o">.</span><span class="n">shape</span><span class="p">[</span><span class="mi">0</span><span class="p">])</span> <span class="o">-</span> <span class="mf">0.01</span><span class="p">,</span> <span class="s1">&#39;+k&#39;</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt output_prompt">Out[138]:</div>




<div class="output_text output_subarea output_execute_result">
<pre>[&lt;matplotlib.lines.Line2D at 0x119cc5b00&gt;]</pre>
</div>

</div>

<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAZIAAAD8CAYAAABdCyJkAAAABHNCSVQICAgIfAhkiAAAAAlwSFlz
AAALEgAACxIB0t1+/AAAEVtJREFUeJzt23+sX3d93/HnC9ylBRSwAxjPjmcknE4wilq+S5i6H1Fx
HG+0TSSoBFOHW1Ddqu3Uif4ySzt7idgCm6BD7egsM+S2aGPKhuKsBffGLGKqCMs1ZVBDwIap9TUO
CTiFpdVGQ9/74x6Tb+6+zrV5X9/jGz8f0lffcz7nfc55n+Or+/L5cVNVSJL07XrG2A1IktY2g0SS
1GKQSJJaDBJJUotBIklqMUgkSS0GiSSpxSCRJLUYJJKklnVjN7Aanv/859e2bdvGbkOS1pRjx459
papesFzdFREk27ZtY35+fuw2JGlNSfLHF1LnrS1JUotBIklqMUgkSS0GiSSpxSCRJLUYJJKkFoNE
ktRikEiSWgwSSVKLQSJJajFIJEktBokkqcUgkSS1GCSSpBaDRJLUYpBIkloMEklSi0EiSWoxSCRJ
LQaJJKnFIJEktRgkkqQWg0SS1GKQSJJaWkGSZEOSuSQnhu/156nbPdScSLJ7avyVST6d5GSSdyfJ
MP6KJB8blt2T5Oqpdb5nWHZ8WP6dnWOQJPV0r0j2AkerajtwdJh/kiQbgH3ADcD1wL6pwHkP8BPA
9uGzaxg/COytqpcDHwR+cdjWOuB3gJ+qqpcBNwJ/0TwGSVJDN0huAQ4N04eAW2fU3AzMVdXZqnoU
mAN2JdkEXF1V91dVAb81tf51wEeH6TngtcP0TuBTVfU/Aarqq1X1zeYxSJIaukGysarODNMPARtn
1GwGTk3NLwxjm4fppeMAx1kMKYAfAa4dpq8DKsmRJJ9I8kvN/iVJTeuWK0hyL/CiGYtum56pqkpS
K9TXm4B3J/lV4DDwjWF8HfC3gb8J/DlwNMmxqjo6o+89wB6ArVu3rlBbkqSllg2SqtpxvmVJvpxk
U1WdGW5VPTyj7DSLzzLO2QLcN4xvWTJ+etjngyzexiLJdcBrhpoF4KNV9ZVh2e8B38fi85mlfR8A
DgBMJpOVCjhJ0hLdW1uHgXNvYe0G7p5RcwTYmWT98JB9J3BkuCX29SSvGt7WeuO59ZO8cPh+BvAr
wG9ObevlSZ41PHj/e8BnmscgSWroBsmdwE1JTgA7hnmSTJIcBKiqs8AdwAPD5/ZhDOCnWXxD6yTw
BeBDw/gbknweeBD4EvC+YVuPAu8ctvNJ4BNV9bvNY5AkNWTxhamnt8lkUvPz82O3IUlryvAMerJc
nX/ZLklqMUgkSS0GiSSpxSCRJLUYJJKkFoNEktRikEiSWgwSSVKLQSJJajFIJEktBokkqcUgkSS1
GCSSpBaDRJLUYpBIkloMEklSi0EiSWoxSCRJLQaJJKnFIJEktRgkkqQWg0SS1GKQSJJaDBJJUotB
IklqMUgkSS0GiSSpxSCRJLUYJJKkFoNEktRikEiSWlpBkmRDkrkkJ4bv9eep2z3UnEiye2r8lUk+
neRkkncnyTD+iiQfG5bdk+TqYfw7khwaxj+b5K2d/iVJfd0rkr3A0araDhwd5p8kyQZgH3ADcD2w
bypw3gP8BLB9+Owaxg8Ce6vq5cAHgV8cxn8EuGoYfyXwk0m2NY9BktTQDZJbgEPD9CHg1hk1NwNz
VXW2qh4F5oBdSTYBV1fV/VVVwG9NrX8d8NFheg547TBdwLOTrAO+C/gG8PXmMUiSGrpBsrGqzgzT
DwEbZ9RsBk5NzS8MY5uH6aXjAMdZDClYvAq5dpi+C/gz4AzwJ8C/rqqzzWOQJDWsW64gyb3Ai2Ys
um16pqoqSa1QX28C3p3kV4HDLF55wOKtsW8CfxVYD/z3JPdW1Rdn9L0H2AOwdevWFWpLkrTUskFS
VTvOtyzJl5Nsqqozw62qh2eUnQZunJrfAtw3jG9ZMn562OeDwM5hH9cBrxlq/iHw4ar6C+DhJH8A
TID/L0iq6gBwAGAymaxUwEmSluje2joMnHsLazdw94yaI8DOJOuHh+w7gSPDLbGvJ3nV8LbWG8+t
n+SFw/czgF8BfnPY1p8APzAsezbwKuDB5jFIkhq6QXIncFOSE8COYZ4kkyQHAYZnGHcADwyf26ee
a/w0i29onQS+AHxoGH9Dks+zGBJfAt43jP8G8Jwkx4dtva+qPtU8BklSQxZfmHp6m0wmNT8/P3Yb
krSmJDlWVZPl6vzLdklSi0EiSWoxSCRJLQaJJKnFIJEktRgkkqQWg0SS1GKQSJJaDBJJUotBIklq
MUgkSS0GiSSpxSCRJLUYJJKkFoNEktRikEiSWgwSSVKLQSJJajFIJEktBokkqcUgkSS1GCSSpBaD
RJLUYpBIkloMEklSi0EiSWoxSCRJLQaJJKnFIJEktRgkkqSWVpAk2ZBkLsmJ4Xv9eep2DzUnkuye
Gn9bklNJHltSf1WSDyQ5meTjSbZNLXvrMP65JDd3+pck9XWvSPYCR6tqO3B0mH+SJBuAfcANwPXA
vqnAuWcYW+rNwKNV9RLgXcDbh229FHg98DJgF/BvkzyzeQzL2r9//6XeRVunx+l118Kxns9q9b7W
ztHY/S63/6XLL7bfc/Wzfo4vdt+XsuZCXMx2LrR2Nf79U1Xf/srJ54Abq+pMkk3AfVX13Utq3jDU
/OQw/++Guv8wVfNYVT1nav4IsL+qPpZkHfAQ8AKGoKqqf7m07qn6nEwmNT8/3zlOOudpNXR6nF53
LRzr+axW72vtHI3d73L7X7r8Yvs9Vz/r5/hi930pay7ExWznQmubvxuOVdVkubruFcnGqjozTD8E
bJxRsxk4NTW/MIw9lW+tU1WPA18Drvk2tyVJuoSWDZIk9yb5oxmfW6brajHyLpv/piXZk2Q+yfwj
jzxy0evv37+fJCQ5tz2SjH6bYFqnx/Ote7HbGdtq/TuthZ+HaWP3u9z+z7f8Qvudtf70erPGltv3
0ltjK1GzEufq26ld7X9/b21d2HFe9rcyvLXlra3zGbtfb21duCv11tZh4NxbWLuBu2fUHAF2Jlmf
xYfsO4exC93u64CPDFc8h4HXZ/GtrhcD24H/0TwGSVJDN0juBG5KcgLYMcyTZJLkIEBVnQXuAB4Y
PrcPYyR5R5IF4FlJFpLsH7b7XuCaJCeBt/DElchx4D8BnwE+DPxMVX2zeQzL2rdv36XeRVunx+l1
18Kxns9q9b7WztHY/S63/6XLL7bfc/Wzfo4vdt+XsuZCXMx2LrR2Nf79W7e21orurS1JuhKt1q0t
SdIVziCRJLUYJJKkFoNEktRikEiSWgwSSVKLQSJJajFIJEktBokkqcUgkSS1GCSSpBaDRJLUYpBI
kloMEklSi0EiSWoxSCRJLQaJJKnFIJEktRgkkqQWg0SS1GKQSJJaDBJJUotBIklqMUgkSS0GiSSp
xSCRJLUYJJKkFoNEktRikEiSWgwSSVKLQSJJamkFSZINSeaSnBi+15+nbvdQcyLJ7qnxtyU5leSx
JfVXJflAkpNJPp5k2zB+U5JjST49fP9Ap39JUl/3imQvcLSqtgNHh/knSbIB2AfcAFwP7JsKnHuG
saXeDDxaVS8B3gW8fRj/CvBDVfVyYDfw283+JUlN3SC5BTg0TB8Cbp1RczMwV1Vnq+pRYA7YBVBV
91fVmWW2exfw6iSpqj+sqi8N48eB70pyVfMYJEkN3SDZOBUEDwEbZ9RsBk5NzS8MY0/lW+tU1ePA
14BrltS8FvhEVf3fi21akrRy1i1XkORe4EUzFt02PVNVlaRWqrFlenoZi7e7dj5FzR5gD8DWrVtX
oy1JuiItGyRVteN8y5J8OcmmqjqTZBPw8Iyy08CNU/NbgPuW2e1p4FpgIck64LnAV4d9bgE+CLyx
qr7wFH0fAA4ATCaTVQk4SboSdW9tHWbxoTfD990zao4AO5OsHx6y7xzGLnS7rwM+MlzxPA/4XWBv
Vf1Bs3dJ0groBsmdwE1JTgA7hnmSTJIcBKiqs8AdwAPD5/ZhjCTvSLIAPCvJQpL9w3bfC1yT5CTw
Fp54G+xngZcA/yzJJ4fPC5vHIElqSNXT/67PZDKp+fn5sduQpDUlybGqmixX51+2S5JaDBJJUotB
IklqMUgkSS0GiSSpxSCRJLUYJJKkFoNEktRikEiSWgwSSVKLQSJJajFIJEktBokkqcUgkSS1GCSS
pBaDRJLUYpBIkloMEklSi0EiSWoxSCRJLQaJJKnFIJEktRgkkqQWg0SS1GKQSJJaDBJJUotBIklq
MUgkSS0GiSSpxSCRJLW0giTJhiRzSU4M3+vPU7d7qDmRZPfU+NuSnEry2JL6q5J8IMnJJB9Psm3J
8q1JHkvyC53+JUl93SuSvcDRqtoOHB3mnyTJBmAfcANwPbBvKnDuGcaWejPwaFW9BHgX8PYly98J
fKjZuyRpBXSD5Bbg0DB9CLh1Rs3NwFxVna2qR4E5YBdAVd1fVWeW2e5dwKuTBCDJrcD/Ao43e5ck
rYBukGycCoKHgI0zajYDp6bmF4axp/KtdarqceBrwDVJngP8MvDPO01LklbOuuUKktwLvGjGotum
Z6qqktRKNXYe+4F3VdVjwwXKeSXZA+wB2Lp16yVuS5KuXMsGSVXtON+yJF9OsqmqziTZBDw8o+w0
cOPU/BbgvmV2exq4FlhIsg54LvBVFp+zvC7JO4DnAX+Z5P9U1a/P6PsAcABgMplc6oCTpCtW99bW
YeDcW1i7gbtn1BwBdiZZPzxk3zmMXeh2Xwd8pBb9naraVlXbgF8D/sWsEJEkrZ5ukNwJ3JTkBLBj
mCfJJMlBgKo6C9wBPDB8bh/GSPKOJAvAs5IsJNk/bPe9LD4TOQm8hRlvg0mSLg+pevrf9ZlMJjU/
Pz92G5K0piQ5VlWT5er8y3ZJUotBIklqMUgkSS0GiSSpxSCRJLUYJJKkFoNEktRikEiSWgwSSVKL
QSJJajFIJEktBokkqcUgkSS1GCSSpBaDRJLUYpBIkloMEklSi0EiSWoxSCRJLQaJJKnFIJEktRgk
kqQWg0SS1GKQSJJaDBJJUkuqauweLrkkjwB/vAq7ej7wlVXYz1rguXiC5+LJPB9PuNzPxV+rqhcs
V3RFBMlqSTJfVZOx+7gceC6e4Ll4Ms/HE54u58JbW5KkFoNEktRikKysA2M3cBnxXDzBc/Fkno8n
PC3Ohc9IJEktXpFIkloMkhWU5F8leTDJp5J8MMnzxu5ptSXZleRzSU4m2Tt2P2NKcm2S/5bkM0mO
J/m5sXsaW5JnJvnDJP917F7GlOR5Se4afl98NsnfGrunDoNkZc0Bf6Oqvgf4PPDWkftZVUmeCfwG
8PeBlwJvSPLScbsa1ePAz1fVS4FXAT9zhZ8PgJ8DPjt2E5eBfwN8uKr+OvAK1vg5MUhWUFX9flU9
PszeD2wZs58RXA+crKovVtU3gP8I3DJyT6OpqjNV9Ylh+n+z+Mti87hdjSfJFuA1wMGxexlTkucC
fxd4L0BVfaOq/nTcrnoMkkvnTcCHxm5ilW0GTk3NL3AF/+KclmQb8L3Ax8ftZFS/BvwS8JdjNzKy
FwOPAO8bbvMdTPLssZvqMEguUpJ7k/zRjM8tUzW3sXhb4/3jdarLRZLnAP8Z+CdV9fWx+xlDkh8E
Hq6qY2P3chlYB3wf8J6q+l7gz4A1/Txx3dgNrDVVteOplif5MeAHgVfXlfdu9Wng2qn5LcPYFSvJ
d7AYIu+vqv8ydj8j+n7gh5P8A+A7gauT/E5V/ejIfY1hAVioqnNXp3exxoPEK5IVlGQXi5fuP1xV
fz52PyN4ANie5MVJ/grweuDwyD2NJklYvA/+2ap659j9jKmq3lpVW6pqG4s/Fx+5QkOEqnoIOJXk
u4ehVwOfGbGlNq9IVtavA1cBc4u/Q7i/qn5q3JZWT1U9nuRngSPAM4F/X1XHR25rTN8P/CPg00k+
OYz906r6vRF70uXhHwPvH/7D9UXgx0fup8W/bJcktXhrS5LUYpBIkloMEklSi0EiSWoxSCRJLQaJ
JKnFIJEktRgkkqSW/wce2FLV5lFElgAAAABJRU5ErkJggg==
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[139]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">fig</span><span class="p">,</span> <span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">subplots</span><span class="p">(</span><span class="mi">2</span><span class="p">,</span> <span class="mi">2</span><span class="p">,</span> <span class="n">sharex</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">sharey</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="c1"># reduce interval of grids</span>
<span class="n">fig</span><span class="o">.</span><span class="n">subplots_adjust</span><span class="p">(</span><span class="n">hspace</span><span class="o">=</span><span class="mf">0.05</span><span class="p">,</span> <span class="n">wspace</span><span class="o">=</span><span class="mf">0.05</span><span class="p">)</span>

<span class="c1">#-------------------------------------------- gerneral kernel ------------------------------------------------#</span>
<span class="c1"># histogram 1</span>
<span class="n">ax</span><span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">]</span><span class="o">.</span><span class="n">hist</span><span class="p">(</span><span class="n">X</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="n">bins</span><span class="o">=</span><span class="n">bins</span><span class="p">,</span> <span class="n">fc</span><span class="o">=</span><span class="s1">&#39;#AAAAFF&#39;</span><span class="p">,</span> <span class="n">normed</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="n">ax</span><span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">]</span><span class="o">.</span><span class="n">text</span><span class="p">(</span><span class="o">-</span><span class="mf">3.5</span><span class="p">,</span> <span class="mf">0.31</span><span class="p">,</span> <span class="s2">&quot;Histogram&quot;</span><span class="p">)</span>

<span class="c1"># histogram 2</span>
<span class="n">ax</span><span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">]</span><span class="o">.</span><span class="n">hist</span><span class="p">(</span><span class="n">X</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="n">bins</span><span class="o">=</span><span class="n">bins</span> <span class="o">+</span> <span class="mf">0.75</span><span class="p">,</span> <span class="n">fc</span><span class="o">=</span><span class="s1">&#39;#AAAAFF&#39;</span><span class="p">,</span> <span class="n">normed</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="n">ax</span><span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">]</span><span class="o">.</span><span class="n">text</span><span class="p">(</span><span class="o">-</span><span class="mf">3.5</span><span class="p">,</span> <span class="mf">0.31</span><span class="p">,</span> <span class="s2">&quot;Histogram, bins shifted&quot;</span><span class="p">)</span>

<span class="c1">#-------------------------------------------- useful kernel ------------------------------------------------#</span>


<span class="c1"># tophat KDE</span>
<span class="n">kde</span> <span class="o">=</span> <span class="n">KernelDensity</span><span class="p">(</span><span class="n">kernel</span><span class="o">=</span><span class="s1">&#39;tophat&#39;</span><span class="p">,</span> <span class="n">bandwidth</span><span class="o">=</span><span class="mf">0.75</span><span class="p">)</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X</span><span class="p">)</span>
<span class="c1"># predicted kernel score</span>
<span class="n">log_dens</span> <span class="o">=</span> <span class="n">kde</span><span class="o">.</span><span class="n">score_samples</span><span class="p">(</span><span class="n">X_plot</span><span class="p">)</span>
<span class="c1"># to become a distribution, take a exponential</span>
<span class="n">ax</span><span class="p">[</span><span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">]</span><span class="o">.</span><span class="n">fill</span><span class="p">(</span><span class="n">X_plot</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="n">np</span><span class="o">.</span><span class="n">exp</span><span class="p">(</span><span class="n">log_dens</span><span class="p">),</span> <span class="n">fc</span><span class="o">=</span><span class="s1">&#39;#AAAAFF&#39;</span><span class="p">)</span>
<span class="c1"># text location</span>
<span class="n">ax</span><span class="p">[</span><span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">]</span><span class="o">.</span><span class="n">text</span><span class="p">(</span><span class="o">-</span><span class="mf">3.5</span><span class="p">,</span> <span class="mf">0.31</span><span class="p">,</span> <span class="s2">&quot;Tophat Kernel Density&quot;</span><span class="p">)</span>

<span class="c1"># Gaussian KDE</span>
<span class="n">kde</span> <span class="o">=</span> <span class="n">KernelDensity</span><span class="p">(</span><span class="n">kernel</span><span class="o">=</span><span class="s1">&#39;gaussian&#39;</span><span class="p">,</span> <span class="n">bandwidth</span><span class="o">=</span><span class="mf">0.75</span><span class="p">)</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X</span><span class="p">)</span>
<span class="n">log_dens</span> <span class="o">=</span> <span class="n">kde</span><span class="o">.</span><span class="n">score_samples</span><span class="p">(</span><span class="n">X_plot</span><span class="p">)</span>
<span class="n">ax</span><span class="p">[</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">]</span><span class="o">.</span><span class="n">fill</span><span class="p">(</span><span class="n">X_plot</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="n">np</span><span class="o">.</span><span class="n">exp</span><span class="p">(</span><span class="n">log_dens</span><span class="p">),</span> <span class="n">fc</span><span class="o">=</span><span class="s1">&#39;#AAAAFF&#39;</span><span class="p">)</span>
<span class="n">ax</span><span class="p">[</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">]</span><span class="o">.</span><span class="n">text</span><span class="p">(</span><span class="o">-</span><span class="mf">3.5</span><span class="p">,</span> <span class="mf">0.31</span><span class="p">,</span> <span class="s2">&quot;Gaussian Kernel Density&quot;</span><span class="p">)</span>

<span class="c1"># [[],[]] -&gt; [,] but for only first axis</span>
<span class="k">for</span> <span class="n">axi</span> <span class="ow">in</span> <span class="n">ax</span><span class="o">.</span><span class="n">ravel</span><span class="p">():</span>
    <span class="c1"># X[:, 0]: flatten X ( + will be expressed as a tick)</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">(</span><span class="n">X</span><span class="o">.</span><span class="n">shape</span><span class="p">[</span><span class="mi">0</span><span class="p">])</span> <span class="o">-</span> <span class="mf">0.01</span><span class="p">,</span> <span class="s1">&#39;+k&#39;</span><span class="p">)</span> <span class="c1"># point</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">set_xlim</span><span class="p">(</span><span class="o">-</span><span class="mi">4</span><span class="p">,</span> <span class="mi">9</span><span class="p">)</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">set_ylim</span><span class="p">(</span><span class="o">-</span><span class="mf">0.02</span><span class="p">,</span> <span class="mf">0.34</span><span class="p">)</span>

<span class="k">for</span> <span class="n">axi</span> <span class="ow">in</span> <span class="n">ax</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">]:</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">set_ylabel</span><span class="p">(</span><span class="s1">&#39;Normalized Density&#39;</span><span class="p">)</span>

<span class="k">for</span> <span class="n">axi</span> <span class="ow">in</span> <span class="n">ax</span><span class="p">[</span><span class="mi">1</span><span class="p">,</span> <span class="p">:]:</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">set_xlabel</span><span class="p">(</span><span class="s1">&#39;x&#39;</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAYUAAAEKCAYAAAD9xUlFAAAABHNCSVQICAgIfAhkiAAAAAlwSFlz
AAALEgAACxIB0t1+/AAAIABJREFUeJzt3XeYVNX5wPHvyyIiUlQgRpquEZRl2QJLlyIgEEEkhKrY
gjWiJiZYHk3kh/GJxigaxUJUsCGxxEgiRFREMaJSpPeyCIgUhaUtsOX9/XFnxtlhZvbu7vR9P88z
z85t574ze2bOnHPuPUdUFWOMMQagRrwDMMYYkzisUDDGGONjhYIxxhgfKxSMMcb4WKFgjDHGxwoF
Y4wxPlYoGGOM8bFCwRhjjI8VCsYYY3xqxjuAimrUqJGec8458Q7DGBYvXrxXVRuH28fyq0kUbvIr
JGGhcM4557Bo0aJ4h2EMIrK1vH0sv5pE4Sa/Qoo1H9WtW7fM8rRp0xg3bhwAzz77LC+//HLIY+fN
m8fnn38e1fiM8Vcd8mt+fj6ZmZlBt1133XWsXr06KucNfG+9/N/XtWvXkpOTQ25uLosXL+bpp5+u
8HkmTJjAX//61yrFmmiSrqZQWTfddFPY7fPmzaNu3bp07dq1yucqLi6mZs1q89aaKKgO+fX555+P
+Tn939d//etfDBs2jPvuu4/8/Hyefvppfv3rX8c8pkSTUjWFcPxL9L/97W9kZGSQlZXFqFGjyM/P
59lnn2XSpEnk5OQwf/588vPz6d27N1lZWfTp04dvvvkGgE2bNtG5c2fatm3Lfffd5/tFMm/ePLp3
787gwYPJyMgAYMiQIbRv3542bdowZcoUXyx169Zl/PjxtGnThr59+/LVV1/Rq1cvzj33XGbOnBnj
d8YkolTKr8XFxVxxxRW0bt2aYcOGceTIEQB69erla1qrW7cu9957L9nZ2XTu3Jldu3YB8Oabb5KZ
mUl2djY9evQ4Ie2dO3fSo0cPcnJyyMzMZP78+b5twdLzvq+zZs3i8ccf55lnnuGiiy7i7rvvZtOm
TeTk5DB+/HgAHnnkETp06EBWVhb333+/L90HH3yQVq1aceGFF7Ju3To3/87koqpJ9Wjfvr2GUqNG
Dc3OzvY9mjdvrrfccouqqt5///36yCOPqKrqWWedpUePHlVV1X379p2wXVV10KBBOm3aNFVVfeGF
F/Syyy5TVdWBAwfq9OnTVVX1mWee0VNPPVVVVT/++GOtU6eObt682ZfG999/r6qqR44c0TZt2uje
vXtVnbHKddasWaqqOmTIEL344ov1+PHjunTpUs3Ozg75+kxiARap5dew79GWLVsU0M8++0xVVa+9
9lpf3D179tSFCxf6zjFz5kxVVR0/frw+8MADqqqamZmp27dvL/Pa/f31r3/VP/3pT6qqWlxcrAcO
HAibnv/75v98y5Yt2qZNG1+677//vl5//fVaWlqqJSUlOnDgQP3kk0900aJFmpmZqYcPH9aCggL9
2c9+Vub/kMjc5FdVLb+mICKPikibqJZMEXLKKaewdOlS32PixIlB98vKyuKKK67g1VdfDVltXrBg
AZdffjkAV155JZ999plv/fDhwwF82706duxIenq6b/lvf/ub75fKtm3b2LBhAwC1atViwIABALRt
25aePXty0kkn0bZtW/Lz8yv/BpikUl3ya/PmzenWrRsAY8aM8cXmr1atWgwaNAiA9u3b+9Lt1q0b
11xzDX//+98pKSk54bgOHTowdepUJkyYwIoVK6hXr17Y9NyaM2cOc+bMITc3l3bt2rF27Vo2bNjA
/Pnz+cUvfkGdOnWoX78+gwcPrlC6ycBN89EaYIqIfCkiN4lIg2gHFW3vvfcet9xyC0uWLKFDhw4U
FxdHJN1TTz3V93zevHl8+OGHLFiwgGXLlpGbm8vRo0cBOOmkkxARAGrUqMHJJ5/sex6pWEzqSPb8
6j021HLgOdLS0nzpPvvss/zpT39i27ZttG/fnu+//77McT169ODTTz+ladOmXHPNNb5O5FDpuaWq
3HPPPb4Ce+PGjYwdO7ZCaSSrcgsFVX1eVbsBVwHnAMtFZLqIXFTesSIyQETWichGEbk7yPabRGSF
iCwVkc9EJKMyL6IiSktL2bZtGxdddBEPP/wwBQUFHDp0iHr16nHw4EHffl27dmXGjBkAvPbaa3Tv
3h2Azp078/bbbwP4tgdTUFDA6aefTp06dVi7di1ffPFFFF+VSVXJkl937NhBnz59gm775ptvWLBg
AQDTp0/nwgsvdJ3upk2b6NSpExMnTqRx48Zs27atzPatW7dy5plncv3113PdddexZMmSCsXtFfh+
9u/fnxdffJFDhw4BzuvbvXs3PXr04F//+heFhYUcPHiQf//735U6XyJz1dEsImnABZ7HXmAZcIeI
hMxlnmMmAz8HMoDRQb70p6tqW1XNAf4CPFbxl1AxJSUljBkzhrZt25Kbm8ttt93GaaedxqWXXso7
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
ad68+Qlp9O3bl1dffZVrrrmGOnXqAE5eWr58ua/m7M1LtWrVomPHjqSnp/uOT09PJycnB3A+b/n5
+Rw6dIjPP/+8TE3y2LFjrt5Lf968vW7dOlauXMnFF18MOJ+3s846y7ef/+c9Pz8fgC5duvDggw+y
fft2hg4dSsuWLcOea/jw4TzwwAM88sgjvPjii1xzzTUVjjcRuGnQWwNMEWe+g6nA66paEN2wYiPY
ULleJ598MgBpaWkUFxcDcO+999K/f39+/etfs3HjxnL7ELzOOussXnrpJfr27csZZ5xBjx49UFWy
srJ8VdVAp556apllbzzwY0ZXVe677z7Gjh1bZl9vvG6sX7+eOnXq0LBhQ1SVp59+mj59+pTZ58MP
Pyxzfu97ctJJJ7Fo0SI++OAD3nzzTZ555hnmzJkT9ny/+tWv+OUvfwk4BV9aWprrWFNJmzZtePvt
t33LkydPZu/evb62/kmTJnHmmWeybNkySktLfU2ONWvWpLS01Hfc0aNHfeu/+uorPvroI9566y2e
euop5s6dy7PPPsuXX37Je++9R/v27Vm8eHGZOP7whz9w0UUX8c4775Cfn0+vXr1824L9z4O58847
eeWVVxg+fDjvvvsuNWvWRFV58sknT+hfmDdvXti8nZaWRmFhIaWlpZx22mllmmMq4+uvvyYvLw9V
pU2bNixYsCDofsE+75dffjmdOnXivffe45JLLuG5556jd+/eIc9Vp04dLr74Yt59913eeOONE97r
ZFFun4KqPq+q3YCrgHOA5SIyXUQuinZwkdC9e3feeecdCgsLOXToEO+++y7du3cHYMuWLSxcuBCA
6dOnc+GFF4ZNq6CggKZNnRlFve244HTeHjx4MOyxF1xwAW+//TajR49m+fLlZGRksGPHDr766isA
jh8/zqpVFauM9e/fnxdeeIHDhw8DTmff3r1uxyyD3bt3c/PNN/vasfv378/TTz/t+1CsW7euTD9L
oIMHD3LgwAEGDRrEpEmT+Prrr0/YJ/C9ad68OY0aNeKhhx5K2l9SkdC7d2+OHj3KM88841t35MgR
3/OCggLOOussatSowSuvvEJJSQng9DusXr2aY8eOsX//fj766CPAqSUWFBRwySWXMGnSJF9b9qZN
m+jUqRMTJ06kcePGbNvmP0Nu6DxdUY8//jj169dn7NixqCr9+/fnmWee8dU81q9f78unbtSvX5/0
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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[140]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Plot all available kernels</span>
<span class="n">X_plot</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="o">-</span><span class="mi">6</span><span class="p">,</span> <span class="mi">6</span><span class="p">,</span> <span class="mi">1000</span><span class="p">)[:,</span> <span class="kc">None</span><span class="p">]</span>
<span class="n">X_src</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">))</span>

<span class="n">fig</span><span class="p">,</span> <span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">subplots</span><span class="p">(</span><span class="mi">2</span><span class="p">,</span> <span class="mi">3</span><span class="p">,</span> <span class="n">sharex</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">sharey</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="c1"># shrink the gap between plots</span>
<span class="n">fig</span><span class="o">.</span><span class="n">subplots_adjust</span><span class="p">(</span><span class="n">left</span><span class="o">=</span><span class="mf">0.05</span><span class="p">,</span> <span class="n">right</span><span class="o">=</span><span class="mf">0.95</span><span class="p">,</span> <span class="n">hspace</span><span class="o">=</span><span class="mf">0.05</span><span class="p">,</span> <span class="n">wspace</span><span class="o">=</span><span class="mf">0.05</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAbUAAAD8CAYAAADwijrNAAAABHNCSVQICAgIfAhkiAAAAAlwSFlz
AAALEgAACxIB0t1+/AAAFclJREFUeJzt3X+sX3d93/HnC6cOW0ohqS8Ssl1ihGlqEFLgKkRDWrOR
CieV7E1MlS2xAs3wuhFUKZQpCJQi94+NMhWpmzswWwpFaoyJJnY3TN0OgiqhOvXNgIATmV4Mre2i
5iak2SpKjKv3/viebzj5cq+/5/qe62sfPx/S1T0/Pvf7eZ/rt76ve77f4/NNVSFJ0hC8YL0LkCSp
L4aaJGkwDDVJ0mAYapKkwTDUJEmDYahJkgZjaqgluT/JE0m+scz+JPmdJAtJHk3yuv7LlCRpui5n
ap8Adl5g/x3A9uZrH/BfVl+WJEkrNzXUqupPgO9dYMhu4Pdr5BjwkiQv66tASZK6uqaHx9gMnG6t
n2m2fXdyYJJ9jM7muO66615/00039TC9rhSPPPLIk1U1M163H65u7X6wFzT5/HCx+gi1zqrqIHAQ
YHZ2tubn5y/l9FpnSf6ivW4/XN3a/WAvaPL54WL1cfXjWWBra31Ls02SpEuqj1CbA365uQryVuCZ
qvqxlx4lSVprU19+TPIAcBuwKckZ4DeAnwCoqo8CR4A7gQXg+8A71qpYSZIuZGqoVdXeKfsLeFdv
FUmSdJG8o4gkaTAMNUnSYBhqkqTBMNQkSYNhqEmSBsNQkyQNhqEmSRoMQ02SNBiGmiRpMAw1SdJg
GGqSpMEw1CRJg9Ep1JLsTHIyyUKSe5fY/zNJHkrylSSPJrmz/1IlSbqwqaGWZANwALgD2AHsTbJj
YtgHgMNVdTOwB/jdvguVJGmaLmdqtwALVXWqqs4Bh4DdE2MK+Klm+cXAX/VXoiRJ3Uz9PDVgM3C6
tX4GeMPEmA8Cf5Tk3cB1wO29VCdJ0gr0daHIXuATVbWF0adgfyrJjz12kn1J5pPMLy4u9jS1rlT2
g8bsBfWlS6idBba21rc029ruAg4DVNWfAi8ENk0+UFUdrKrZqpqdmZm5uIo1GPaDxuwF9aVLqB0H
tifZlmQjowtB5ibG/CXwJoAkP8co1PxzS5J0SU0Ntao6D9wNHAUeZ3SV44kk+5Psaoa9B3hnkq8B
DwBvr6paq6IlSVpKlwtFqKojwJGJbfe1lh8D3thvaZIkrYx3FJEkDYahJkkaDENNkjQYhpokaTAM
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
GIaaJGkw/j9vgQ/0DP+WWwAAAABJRU5ErkJggg==
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[96]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Plot all available kernels</span>
<span class="n">X_plot</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="o">-</span><span class="mi">6</span><span class="p">,</span> <span class="mi">6</span><span class="p">,</span> <span class="mi">1000</span><span class="p">)[:,</span> <span class="kc">None</span><span class="p">]</span>
<span class="n">X_src</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">))</span>

<span class="n">fig</span><span class="p">,</span> <span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">subplots</span><span class="p">(</span><span class="mi">2</span><span class="p">,</span> <span class="mi">3</span><span class="p">,</span> <span class="n">sharex</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">sharey</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="n">fig</span><span class="o">.</span><span class="n">subplots_adjust</span><span class="p">(</span><span class="n">left</span><span class="o">=</span><span class="mf">0.05</span><span class="p">,</span> <span class="n">right</span><span class="o">=</span><span class="mf">0.95</span><span class="p">,</span> <span class="n">hspace</span><span class="o">=</span><span class="mf">0.05</span><span class="p">,</span> <span class="n">wspace</span><span class="o">=</span><span class="mf">0.05</span><span class="p">)</span>


<span class="k">def</span> <span class="nf">format_func</span><span class="p">(</span><span class="n">x</span><span class="p">,</span> <span class="n">loc</span><span class="p">):</span>
    <span class="k">if</span> <span class="n">x</span> <span class="o">==</span> <span class="mi">0</span><span class="p">:</span>
        <span class="k">return</span> <span class="s1">&#39;0&#39;</span>
    <span class="k">elif</span> <span class="n">x</span> <span class="o">==</span> <span class="mi">1</span><span class="p">:</span>
        <span class="k">return</span> <span class="s1">&#39;h&#39;</span>
    <span class="k">elif</span> <span class="n">x</span> <span class="o">==</span> <span class="o">-</span><span class="mi">1</span><span class="p">:</span>
        <span class="k">return</span> <span class="s1">&#39;-h&#39;</span>
    <span class="k">else</span><span class="p">:</span>
        <span class="k">return</span> <span class="s1">&#39;</span><span class="si">%i</span><span class="s1">h&#39;</span> <span class="o">%</span> <span class="n">x</span>

<span class="k">for</span> <span class="n">i</span><span class="p">,</span> <span class="n">kernel</span> <span class="ow">in</span> <span class="nb">enumerate</span><span class="p">([</span><span class="s1">&#39;gaussian&#39;</span><span class="p">,</span> <span class="s1">&#39;tophat&#39;</span><span class="p">,</span> <span class="s1">&#39;epanechnikov&#39;</span><span class="p">,</span>
                            <span class="s1">&#39;exponential&#39;</span><span class="p">,</span> <span class="s1">&#39;linear&#39;</span><span class="p">,</span> <span class="s1">&#39;cosine&#39;</span><span class="p">]):</span>
    <span class="c1"># flatten plot lists</span>
    <span class="n">axi</span> <span class="o">=</span> <span class="n">ax</span><span class="o">.</span><span class="n">ravel</span><span class="p">()[</span><span class="n">i</span><span class="p">]</span>
    <span class="c1"># with specific kernel</span>
    <span class="n">log_dens</span> <span class="o">=</span> <span class="n">KernelDensity</span><span class="p">(</span><span class="n">kernel</span><span class="o">=</span><span class="n">kernel</span><span class="p">)</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X_src</span><span class="p">)</span><span class="o">.</span><span class="n">score_samples</span><span class="p">(</span><span class="n">X_plot</span><span class="p">)</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">fill</span><span class="p">(</span><span class="n">X_plot</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="n">np</span><span class="o">.</span><span class="n">exp</span><span class="p">(</span><span class="n">log_dens</span><span class="p">),</span> <span class="s1">&#39;-k&#39;</span><span class="p">,</span> <span class="n">fc</span><span class="o">=</span><span class="s1">&#39;#AAAAFF&#39;</span><span class="p">)</span>
    <span class="c1"># transfer text</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">text</span><span class="p">(</span><span class="o">-</span><span class="mf">2.6</span><span class="p">,</span> <span class="mf">0.95</span><span class="p">,</span> <span class="n">kernel</span><span class="p">)</span>
    
    <span class="c1"># plt.FuncFormatter: x ticks expressed by some of strings</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">xaxis</span><span class="o">.</span><span class="n">set_major_formatter</span><span class="p">(</span><span class="n">plt</span><span class="o">.</span><span class="n">FuncFormatter</span><span class="p">(</span><span class="n">format_func</span><span class="p">))</span>
    <span class="c1"># express per a tick </span>
    <span class="n">axi</span><span class="o">.</span><span class="n">xaxis</span><span class="o">.</span><span class="n">set_major_locator</span><span class="p">(</span><span class="n">plt</span><span class="o">.</span><span class="n">MultipleLocator</span><span class="p">(</span><span class="mi">1</span><span class="p">))</span>
    <span class="c1"># remove y ticks</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">yaxis</span><span class="o">.</span><span class="n">set_major_locator</span><span class="p">(</span><span class="n">plt</span><span class="o">.</span><span class="n">NullLocator</span><span class="p">())</span>
    <span class="c1"># range of x,y</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">set_ylim</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mf">1.05</span><span class="p">)</span>
    <span class="n">axi</span><span class="o">.</span><span class="n">set_xlim</span><span class="p">(</span><span class="o">-</span><span class="mf">2.9</span><span class="p">,</span> <span class="mf">2.9</span><span class="p">)</span>
    
<span class="c1"># text will be located above [0,1]</span>
<span class="n">ax</span><span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">]</span><span class="o">.</span><span class="n">set_title</span><span class="p">(</span><span class="s1">&#39;Available Kernels&#39;</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt output_prompt">Out[96]:</div>




<div class="output_text output_subarea output_execute_result">
<pre>&lt;matplotlib.text.Text at 0x118f15518&gt;</pre>
</div>

</div>

<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[53]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1">#----------------------------------------------------------------------</span>
<span class="c1"># Plot a 1D density example</span>
<span class="n">N</span> <span class="o">=</span> <span class="mi">100</span>
<span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">seed</span><span class="p">(</span><span class="mi">1</span><span class="p">)</span>
<span class="n">X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">((</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">normal</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="nb">int</span><span class="p">(</span><span class="mf">0.3</span> <span class="o">*</span> <span class="n">N</span><span class="p">)),</span>
                    <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">normal</span><span class="p">(</span><span class="mi">5</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="nb">int</span><span class="p">(</span><span class="mf">0.7</span> <span class="o">*</span> <span class="n">N</span><span class="p">))))[:,</span> <span class="n">np</span><span class="o">.</span><span class="n">newaxis</span><span class="p">]</span>

<span class="n">X_plot</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="o">-</span><span class="mi">5</span><span class="p">,</span> <span class="mi">10</span><span class="p">,</span> <span class="mi">1000</span><span class="p">)[:,</span> <span class="n">np</span><span class="o">.</span><span class="n">newaxis</span><span class="p">]</span>

<span class="n">true_dens</span> <span class="o">=</span> <span class="p">(</span><span class="mf">0.3</span> <span class="o">*</span> <span class="n">norm</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span><span class="o">.</span><span class="n">pdf</span><span class="p">(</span><span class="n">X_plot</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">])</span>
             <span class="o">+</span> <span class="mf">0.7</span> <span class="o">*</span> <span class="n">norm</span><span class="p">(</span><span class="mi">5</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span><span class="o">.</span><span class="n">pdf</span><span class="p">(</span><span class="n">X_plot</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">]))</span>

<span class="n">fig</span><span class="p">,</span> <span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">subplots</span><span class="p">()</span>
<span class="n">ax</span><span class="o">.</span><span class="n">fill</span><span class="p">(</span><span class="n">X_plot</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="n">true_dens</span><span class="p">,</span> <span class="n">fc</span><span class="o">=</span><span class="s1">&#39;black&#39;</span><span class="p">,</span> <span class="n">alpha</span><span class="o">=</span><span class="mf">0.2</span><span class="p">,</span>
        <span class="n">label</span><span class="o">=</span><span class="s1">&#39;input distribution&#39;</span><span class="p">)</span>

<span class="k">for</span> <span class="n">kernel</span> <span class="ow">in</span> <span class="p">[</span><span class="s1">&#39;gaussian&#39;</span><span class="p">,</span> <span class="s1">&#39;tophat&#39;</span><span class="p">,</span> <span class="s1">&#39;epanechnikov&#39;</span><span class="p">]:</span>
    <span class="n">kde</span> <span class="o">=</span> <span class="n">KernelDensity</span><span class="p">(</span><span class="n">kernel</span><span class="o">=</span><span class="n">kernel</span><span class="p">,</span> <span class="n">bandwidth</span><span class="o">=</span><span class="mf">0.5</span><span class="p">)</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X</span><span class="p">)</span>
    <span class="n">log_dens</span> <span class="o">=</span> <span class="n">kde</span><span class="o">.</span><span class="n">score_samples</span><span class="p">(</span><span class="n">X_plot</span><span class="p">)</span>
    <span class="n">ax</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X_plot</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="n">np</span><span class="o">.</span><span class="n">exp</span><span class="p">(</span><span class="n">log_dens</span><span class="p">),</span> <span class="s1">&#39;-&#39;</span><span class="p">,</span>
            <span class="n">label</span><span class="o">=</span><span class="s2">&quot;kernel = &#39;</span><span class="si">{0}</span><span class="s2">&#39;&quot;</span><span class="o">.</span><span class="n">format</span><span class="p">(</span><span class="n">kernel</span><span class="p">))</span>

<span class="n">ax</span><span class="o">.</span><span class="n">text</span><span class="p">(</span><span class="mi">6</span><span class="p">,</span> <span class="mf">0.38</span><span class="p">,</span> <span class="s2">&quot;N=</span><span class="si">{0}</span><span class="s2"> points&quot;</span><span class="o">.</span><span class="n">format</span><span class="p">(</span><span class="n">N</span><span class="p">))</span>

<span class="n">ax</span><span class="o">.</span><span class="n">legend</span><span class="p">(</span><span class="n">loc</span><span class="o">=</span><span class="s1">&#39;upper left&#39;</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">X</span><span class="p">[:,</span> <span class="mi">0</span><span class="p">],</span> <span class="o">-</span><span class="mf">0.005</span> <span class="o">-</span> <span class="mf">0.01</span> <span class="o">*</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">random</span><span class="p">(</span><span class="n">X</span><span class="o">.</span><span class="n">shape</span><span class="p">[</span><span class="mi">0</span><span class="p">]),</span> <span class="s1">&#39;+k&#39;</span><span class="p">)</span>

<span class="n">ax</span><span class="o">.</span><span class="n">set_xlim</span><span class="p">(</span><span class="o">-</span><span class="mi">4</span><span class="p">,</span> <span class="mi">9</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">set_ylim</span><span class="p">(</span><span class="o">-</span><span class="mf">0.02</span><span class="p">,</span> <span class="mf">0.4</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
rkJggg==
"
>
</div>

</div>

</div>
</div>


</div>
</div>
</body>

 


</html>



