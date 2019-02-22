<!DOCTYPE html>
<!-- saved from url=(0144)http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let's-do-the-predictions-for-our-test-data-and-print-the-result-as-given-below -->
<html lang="fr-fr"><head><meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    

    <title>Mammographic_Masses_Dataset</title>
    
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <link rel="stylesheet" href="./README_files/jquery-ui.min.css" type="text/css">
    <link rel="stylesheet" href="./README_files/jquery.typeahead.min.css" type="text/css">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    


<script type="text/javascript" src="./README_files/MathJax.js.download" charset="utf-8"></script>

<script type="text/javascript">
// MathJax disabled, set as null to distingish from *missing* MathJax,
// where it will be undefined, and should prompt a dialog later.
window.mathjax_url = "/static/components/MathJax/MathJax.js";
</script>

<link rel="stylesheet" href="./README_files/bootstrap-tour.min.css" type="text/css">
<link rel="stylesheet" href="./README_files/codemirror.css">


    <link rel="stylesheet" href="./README_files/style.min.css" type="text/css">
    

<link rel="stylesheet" href="./README_files/override.css" type="text/css">
<link rel="stylesheet" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb" id="kernel-css" type="text/css">


    <link rel="stylesheet" href="./README_files/custom.css" type="text/css">
    <script src="./README_files/promise.min.js.download" type="text/javascript" charset="utf-8"></script>
    <script src="./README_files/index.js.download" type="text/javascript"></script>
    <script src="./README_files/index.js(1).download" type="text/javascript"></script>
    <script src="./README_files/index.js(2).download" type="text/javascript"></script>
    <script src="./README_files/require.js.download" type="text/javascript" charset="utf-8"></script>
    <script>
      require.config({
          
          urlArgs: "v=20190222144558",
          
          baseUrl: '/static/',
          paths: {
            'auth/js/main': 'auth/js/main.min',
            custom : '/custom',
            nbextensions : '/nbextensions',
            kernelspecs : '/kernelspecs',
            underscore : 'components/underscore/underscore-min',
            backbone : 'components/backbone/backbone-min',
            jed: 'components/jed/jed',
            jquery: 'components/jquery/jquery.min',
            json: 'components/requirejs-plugins/src/json',
            text: 'components/requirejs-text/text',
            bootstrap: 'components/bootstrap/js/bootstrap.min',
            bootstraptour: 'components/bootstrap-tour/build/js/bootstrap-tour.min',
            'jquery-ui': 'components/jquery-ui/ui/minified/jquery-ui.min',
            moment: 'components/moment/min/moment-with-locales',
            codemirror: 'components/codemirror',
            termjs: 'components/xterm.js/xterm',
            typeahead: 'components/jquery-typeahead/dist/jquery.typeahead.min',
          },
          map: { // for backward compatibility
              "*": {
                  "jqueryui": "jquery-ui",
              }
          },
          shim: {
            typeahead: {
              deps: ["jquery"],
              exports: "typeahead"
            },
            underscore: {
              exports: '_'
            },
            backbone: {
              deps: ["underscore", "jquery"],
              exports: "Backbone"
            },
            bootstrap: {
              deps: ["jquery"],
              exports: "bootstrap"
            },
            bootstraptour: {
              deps: ["bootstrap"],
              exports: "Tour"
            },
            "jquery-ui": {
              deps: ["jquery"],
              exports: "$"
            }
          },
          waitSeconds: 30,
      });

      require.config({
          map: {
              '*':{
                'contents': 'services/contents',
              }
          }
      });

      // error-catching custom.js shim.
      define("custom", function (require, exports, module) {
          try {
              var custom = require('custom/custom');
              console.debug('loaded custom.js');
              return custom;
          } catch (e) {
              console.error("error loading custom.js", e);
              return {};
          }
      })

    document.nbjs_translations = {"domain": "nbjs", "locale_data": {"nbjs": {"": {"domain": "nbjs"}}}};
    document.documentElement.lang = navigator.language.toLowerCase();
    </script>

    
    

<script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="services/contents" src="./README_files/contents.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="custom/custom" src="./README_files/custom.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/plotlywidget/extension" src="./README_files/extension.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/jupyter-js-widgets/extension" src="./README_files/extension.js(1).download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/nbextensions_configurator/config_menu/main" src="./README_files/main.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/contrib_nbextensions_help_item/main" src="./README_files/main.js(1).download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/varInspector/main" src="./README_files/main.js(2).download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/code_prettify/code_prettify" src="./README_files/code_prettify.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/collapsible_headings/main" src="./README_files/main.js(3).download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/toc2/main" src="./README_files/main.js(4).download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/codefolding/main" src="./README_files/main.js(5).download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/export_embedded/main" src="./README_files/main.js(6).download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/code_prettify/autopep8" src="./README_files/autopep8.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/code_font_size/code_font_size" src="./README_files/code_font_size.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/gist_it/main" src="./README_files/main.js(7).download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/python-markdown/main" src="./README_files/main.js(8).download"></script><style type="text/css">.MathJax_Hover_Frame {border-radius: .25em; -webkit-border-radius: .25em; -moz-border-radius: .25em; -khtml-border-radius: .25em; box-shadow: 0px 0px 15px #83A; -webkit-box-shadow: 0px 0px 15px #83A; -moz-box-shadow: 0px 0px 15px #83A; -khtml-box-shadow: 0px 0px 15px #83A; border: 1px solid #A6D ! important; display: inline-block; position: absolute}
.MathJax_Menu_Button .MathJax_Hover_Arrow {position: absolute; cursor: pointer; display: inline-block; border: 2px solid #AAA; border-radius: 4px; -webkit-border-radius: 4px; -moz-border-radius: 4px; -khtml-border-radius: 4px; font-family: 'Courier New',Courier; font-size: 9px; color: #F0F0F0}
.MathJax_Menu_Button .MathJax_Hover_Arrow span {display: block; background-color: #AAA; border: 1px solid; border-radius: 3px; line-height: 0; padding: 4px}
.MathJax_Hover_Arrow:hover {color: white!important; border: 2px solid #CCC!important}
.MathJax_Hover_Arrow:hover span {background-color: #CCC!important}
</style><style type="text/css">#MathJax_About {position: fixed; left: 50%; width: auto; text-align: center; border: 3px outset; padding: 1em 2em; background-color: #DDDDDD; color: black; cursor: default; font-family: message-box; font-size: 120%; font-style: normal; text-indent: 0; text-transform: none; line-height: normal; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; z-index: 201; border-radius: 15px; -webkit-border-radius: 15px; -moz-border-radius: 15px; -khtml-border-radius: 15px; box-shadow: 0px 10px 20px #808080; -webkit-box-shadow: 0px 10px 20px #808080; -moz-box-shadow: 0px 10px 20px #808080; -khtml-box-shadow: 0px 10px 20px #808080; filter: progid:DXImageTransform.Microsoft.dropshadow(OffX=2, OffY=2, Color='gray', Positive='true')}
#MathJax_About.MathJax_MousePost {outline: none}
.MathJax_Menu {position: absolute; background-color: white; color: black; width: auto; padding: 2px; border: 1px solid #CCCCCC; margin: 0; cursor: default; font: menu; text-align: left; text-indent: 0; text-transform: none; line-height: normal; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; z-index: 201; box-shadow: 0px 10px 20px #808080; -webkit-box-shadow: 0px 10px 20px #808080; -moz-box-shadow: 0px 10px 20px #808080; -khtml-box-shadow: 0px 10px 20px #808080; filter: progid:DXImageTransform.Microsoft.dropshadow(OffX=2, OffY=2, Color='gray', Positive='true')}
.MathJax_MenuItem {padding: 2px 2em; background: transparent}
.MathJax_MenuArrow {position: absolute; right: .5em; padding-top: .25em; color: #666666; font-size: .75em}
.MathJax_MenuActive .MathJax_MenuArrow {color: white}
.MathJax_MenuArrow.RTL {left: .5em; right: auto}
.MathJax_MenuCheck {position: absolute; left: .7em}
.MathJax_MenuCheck.RTL {right: .7em; left: auto}
.MathJax_MenuRadioCheck {position: absolute; left: 1em}
.MathJax_MenuRadioCheck.RTL {right: 1em; left: auto}
.MathJax_MenuLabel {padding: 2px 2em 4px 1.33em; font-style: italic}
.MathJax_MenuRule {border-top: 1px solid #CCCCCC; margin: 4px 1px 0px}
.MathJax_MenuDisabled {color: GrayText}
.MathJax_MenuActive {background-color: Highlight; color: HighlightText}
.MathJax_MenuDisabled:focus, .MathJax_MenuLabel:focus {background-color: #E8E8E8}
.MathJax_ContextMenu:focus {outline: none}
.MathJax_ContextMenu .MathJax_MenuItem:focus {outline: none}
#MathJax_AboutClose {top: .2em; right: .2em}
.MathJax_Menu .MathJax_MenuClose {top: -10px; left: -10px}
.MathJax_MenuClose {position: absolute; cursor: pointer; display: inline-block; border: 2px solid #AAA; border-radius: 18px; -webkit-border-radius: 18px; -moz-border-radius: 18px; -khtml-border-radius: 18px; font-family: 'Courier New',Courier; font-size: 24px; color: #F0F0F0}
.MathJax_MenuClose span {display: block; background-color: #AAA; border: 1.5px solid; border-radius: 18px; -webkit-border-radius: 18px; -moz-border-radius: 18px; -khtml-border-radius: 18px; line-height: 0; padding: 8px 0 6px}
.MathJax_MenuClose:hover {color: white!important; border: 2px solid #CCC!important}
.MathJax_MenuClose:hover span {background-color: #CCC!important}
.MathJax_MenuClose:hover:focus {outline: none}
</style><style type="text/css">.MathJax_Preview .MJXf-math {color: inherit!important}
</style><style type="text/css">.MJX_Assistive_MathML {position: absolute!important; top: 0; left: 0; clip: rect(1px, 1px, 1px, 1px); padding: 1px 0 0 0!important; border: 0!important; height: 1px!important; width: 1px!important; overflow: hidden!important; display: block!important; -webkit-touch-callout: none; -webkit-user-select: none; -khtml-user-select: none; -moz-user-select: none; -ms-user-select: none; user-select: none}
.MJX_Assistive_MathML.MJX_Assistive_MathML_Block {width: 100%!important}
</style><style type="text/css">#MathJax_Zoom {position: absolute; background-color: #F0F0F0; overflow: auto; display: block; z-index: 301; padding: .5em; border: 1px solid black; margin: 0; font-weight: normal; font-style: normal; text-align: left; text-indent: 0; text-transform: none; line-height: normal; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; -webkit-box-sizing: content-box; -moz-box-sizing: content-box; box-sizing: content-box; box-shadow: 5px 5px 15px #AAAAAA; -webkit-box-shadow: 5px 5px 15px #AAAAAA; -moz-box-shadow: 5px 5px 15px #AAAAAA; -khtml-box-shadow: 5px 5px 15px #AAAAAA; filter: progid:DXImageTransform.Microsoft.dropshadow(OffX=2, OffY=2, Color='gray', Positive='true')}
#MathJax_ZoomOverlay {position: absolute; left: 0; top: 0; z-index: 300; display: inline-block; width: 100%; height: 100%; border: 0; padding: 0; margin: 0; background-color: white; opacity: 0; filter: alpha(opacity=0)}
#MathJax_ZoomFrame {position: relative; display: inline-block; height: 0; width: 0}
#MathJax_ZoomEventTrap {position: absolute; left: 0; top: 0; z-index: 302; display: inline-block; border: 0; padding: 0; margin: 0; background-color: white; opacity: 0; filter: alpha(opacity=0)}
</style><style type="text/css">.MathJax_Preview {color: #888}
#MathJax_Message {position: fixed; left: 1em; bottom: 1.5em; background-color: #E6E6E6; border: 1px solid #959595; margin: 0px; padding: 2px 8px; z-index: 102; color: black; font-size: 80%; width: auto; white-space: nowrap}
#MathJax_MSIE_Frame {position: absolute; top: 0; left: 0; width: 0px; z-index: 101; border: 0px; margin: 0px; padding: 0px}
.MathJax_Error {color: #CC0000; font-style: italic}
</style><link type="text/css" rel="stylesheet" href="./README_files/main.css"><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/code_prettify/kernel_exec_on_cell" src="./README_files/kernel_exec_on_cell.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="codemirror/addon/fold/foldcode" src="./README_files/foldcode.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="codemirror/addon/fold/foldgutter" src="./README_files/foldgutter.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="codemirror/addon/fold/brace-fold" src="./README_files/brace-fold.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="codemirror/addon/fold/indent-fold" src="./README_files/indent-fold.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/toc2/toc2" src="./README_files/toc2.js.download"></script><link id="collapsible_headings_css" rel="stylesheet" type="text/css" href="./README_files/main(1).css"><link type="text/css" rel="stylesheet" href="./README_files/main(2).css"><style type="text/css">div.MathJax_MathML {text-align: center; margin: .75em 0px; display: block!important}
.MathJax_MathML {font-style: normal; font-weight: normal; line-height: normal; font-size: 100%; font-size-adjust: none; text-indent: 0; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0; min-height: 0; border: 0; padding: 0; margin: 0}
span.MathJax_MathML {display: inline!important}
.MathJax_mmlExBox {display: block!important; overflow: hidden; height: 1px; width: 60ex; min-height: 0; max-height: none; padding: 0; border: 0; margin: 0}
[class="MJX-tex-oldstyle"] {font-family: MathJax_Caligraphic, MathJax_Caligraphic-WEB}
[class="MJX-tex-oldstyle-bold"] {font-family: MathJax_Caligraphic, MathJax_Caligraphic-WEB; font-weight: bold}
[class="MJX-tex-caligraphic"] {font-family: MathJax_Caligraphic, MathJax_Caligraphic-WEB}
[class="MJX-tex-caligraphic-bold"] {font-family: MathJax_Caligraphic, MathJax_Caligraphic-WEB; font-weight: bold}
@font-face /*1*/ {font-family: MathJax_Caligraphic-WEB; src: url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/TeX/otf/MathJax_Caligraphic-Regular.otf')}
@font-face /*2*/ {font-family: MathJax_Caligraphic-WEB; font-weight: bold; src: url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/TeX/otf/MathJax_Caligraphic-Bold.otf')}
[mathvariant="double-struck"] {font-family: MathJax_AMS, MathJax_AMS-WEB}
[mathvariant="script"] {font-family: MathJax_Script, MathJax_Script-WEB}
[mathvariant="fraktur"] {font-family: MathJax_Fraktur, MathJax_Fraktur-WEB}
[mathvariant="bold-script"] {font-family: MathJax_Script, MathJax_Caligraphic-WEB; font-weight: bold}
[mathvariant="bold-fraktur"] {font-family: MathJax_Fraktur, MathJax_Fraktur-WEB; font-weight: bold}
[mathvariant="monospace"] {font-family: monospace}
[mathvariant="sans-serif"] {font-family: sans-serif}
[mathvariant="bold-sans-serif"] {font-family: sans-serif; font-weight: bold}
[mathvariant="sans-serif-italic"] {font-family: sans-serif; font-style: italic}
[mathvariant="sans-serif-bold-italic"] {font-family: sans-serif; font-style: italic; font-weight: bold}
@font-face /*3*/ {font-family: MathJax_AMS-WEB; src: url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/TeX/otf/MathJax_AMS-Regular.otf')}
@font-face /*4*/ {font-family: MathJax_Script-WEB; src: url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/TeX/otf/MathJax_Script-Regular.otf')}
@font-face /*5*/ {font-family: MathJax_Fraktur-WEB; src: url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/TeX/otf/MathJax_Fraktur-Regular.otf')}
@font-face /*6*/ {font-family: MathJax_Fraktur-WEB; font-weight: bold; src: url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/TeX/otf/MathJax_Fraktur-Bold.otf')}
</style><style type="text/css">.MJXp-script {font-size: .8em}
.MJXp-right {-webkit-transform-origin: right; -moz-transform-origin: right; -ms-transform-origin: right; -o-transform-origin: right; transform-origin: right}
.MJXp-bold {font-weight: bold}
.MJXp-italic {font-style: italic}
.MJXp-scr {font-family: MathJax_Script,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-frak {font-family: MathJax_Fraktur,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-sf {font-family: MathJax_SansSerif,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-cal {font-family: MathJax_Caligraphic,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-mono {font-family: MathJax_Typewriter,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-largeop {font-size: 150%}
.MJXp-largeop.MJXp-int {vertical-align: -.2em}
.MJXp-math {display: inline-block; line-height: 1.2; text-indent: 0; font-family: 'Times New Roman',Times,STIXGeneral,serif; white-space: nowrap; border-collapse: collapse}
.MJXp-display {display: block; text-align: center; margin: 1em 0}
.MJXp-math span {display: inline-block}
.MJXp-box {display: block!important; text-align: center}
.MJXp-box:after {content: " "}
.MJXp-rule {display: block!important; margin-top: .1em}
.MJXp-char {display: block!important}
.MJXp-mo {margin: 0 .15em}
.MJXp-mfrac {margin: 0 .125em; vertical-align: .25em}
.MJXp-denom {display: inline-table!important; width: 100%}
.MJXp-denom > * {display: table-row!important}
.MJXp-surd {vertical-align: top}
.MJXp-surd > * {display: block!important}
.MJXp-script-box > *  {display: table!important; height: 50%}
.MJXp-script-box > * > * {display: table-cell!important; vertical-align: top}
.MJXp-script-box > *:last-child > * {vertical-align: bottom}
.MJXp-script-box > * > * > * {display: block!important}
.MJXp-mphantom {visibility: hidden}
.MJXp-munderover {display: inline-table!important}
.MJXp-over {display: inline-block!important; text-align: center}
.MJXp-over > * {display: block!important}
.MJXp-munderover > * {display: table-row!important}
.MJXp-mtable {vertical-align: .25em; margin: 0 .125em}
.MJXp-mtable > * {display: inline-table!important; vertical-align: middle}
.MJXp-mtr {display: table-row!important}
.MJXp-mtd {display: table-cell!important; text-align: center; padding: .5em 0 0 .5em}
.MJXp-mtr > .MJXp-mtd:first-child {padding-left: 0}
.MJXp-mtr:first-child > .MJXp-mtd {padding-top: 0}
.MJXp-mlabeledtr {display: table-row!important}
.MJXp-mlabeledtr > .MJXp-mtd:first-child {padding-left: 0}
.MJXp-mlabeledtr:first-child > .MJXp-mtd {padding-top: 0}
.MJXp-merror {background-color: #FFFF88; color: #CC0000; border: 1px solid #CC0000; padding: 1px 3px; font-style: normal; font-size: 90%}
.MJXp-scale0 {-webkit-transform: scaleX(.0); -moz-transform: scaleX(.0); -ms-transform: scaleX(.0); -o-transform: scaleX(.0); transform: scaleX(.0)}
.MJXp-scale1 {-webkit-transform: scaleX(.1); -moz-transform: scaleX(.1); -ms-transform: scaleX(.1); -o-transform: scaleX(.1); transform: scaleX(.1)}
.MJXp-scale2 {-webkit-transform: scaleX(.2); -moz-transform: scaleX(.2); -ms-transform: scaleX(.2); -o-transform: scaleX(.2); transform: scaleX(.2)}
.MJXp-scale3 {-webkit-transform: scaleX(.3); -moz-transform: scaleX(.3); -ms-transform: scaleX(.3); -o-transform: scaleX(.3); transform: scaleX(.3)}
.MJXp-scale4 {-webkit-transform: scaleX(.4); -moz-transform: scaleX(.4); -ms-transform: scaleX(.4); -o-transform: scaleX(.4); transform: scaleX(.4)}
.MJXp-scale5 {-webkit-transform: scaleX(.5); -moz-transform: scaleX(.5); -ms-transform: scaleX(.5); -o-transform: scaleX(.5); transform: scaleX(.5)}
.MJXp-scale6 {-webkit-transform: scaleX(.6); -moz-transform: scaleX(.6); -ms-transform: scaleX(.6); -o-transform: scaleX(.6); transform: scaleX(.6)}
.MJXp-scale7 {-webkit-transform: scaleX(.7); -moz-transform: scaleX(.7); -ms-transform: scaleX(.7); -o-transform: scaleX(.7); transform: scaleX(.7)}
.MJXp-scale8 {-webkit-transform: scaleX(.8); -moz-transform: scaleX(.8); -ms-transform: scaleX(.8); -o-transform: scaleX(.8); transform: scaleX(.8)}
.MJXp-scale9 {-webkit-transform: scaleX(.9); -moz-transform: scaleX(.9); -ms-transform: scaleX(.9); -o-transform: scaleX(.9); transform: scaleX(.9)}
.MathJax_PHTML .noError {vertical-align: ; font-size: 90%; text-align: left; color: black; padding: 1px 3px; border: 1px solid}
</style><style id="collapsible_headings_indent_css">.collapsible_headings_toggle .h1 { margin-right: 40px; }
.collapsible_headings_toggle .h2 { margin-right: 32px; }
.collapsible_headings_toggle .h3 { margin-right: 24px; }
.collapsible_headings_toggle .h4 { margin-right: 16px; }
.collapsible_headings_toggle .h5 { margin-right: 8px; }
.collapsible_headings_toggle .h6 { margin-right: 0px; }</style><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/varInspector/jquery.tablesorter.min" src="./README_files/jquery.tablesorter.min.js.download"></script><style type="text/css">.MathJax_Display {text-align: center; margin: 0; position: relative; display: block!important; text-indent: 0; max-width: none; max-height: none; min-width: 0; min-height: 0; width: 100%}
.MathJax .merror {background-color: #FFFF88; color: #CC0000; border: 1px solid #CC0000; padding: 1px 3px; font-style: normal; font-size: 90%}
.MathJax .MJX-monospace {font-family: monospace}
.MathJax .MJX-sans-serif {font-family: sans-serif}
#MathJax_Tooltip {background-color: InfoBackground; color: InfoText; border: 1px solid black; box-shadow: 2px 2px 5px #AAAAAA; -webkit-box-shadow: 2px 2px 5px #AAAAAA; -moz-box-shadow: 2px 2px 5px #AAAAAA; -khtml-box-shadow: 2px 2px 5px #AAAAAA; filter: progid:DXImageTransform.Microsoft.dropshadow(OffX=2, OffY=2, Color='gray', Positive='true'); padding: 3px 4px; z-index: 401; position: absolute; left: 0; top: 0; width: auto; height: auto; display: none}
.MathJax {display: inline; font-style: normal; font-weight: normal; line-height: normal; font-size: 100%; font-size-adjust: none; text-indent: 0; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0; min-height: 0; border: 0; padding: 0; margin: 0}
.MathJax:focus, body :focus .MathJax {display: inline-table}
.MathJax.MathJax_FullWidth {text-align: center; display: table-cell!important; width: 10000em!important}
.MathJax img, .MathJax nobr, .MathJax a {border: 0; padding: 0; margin: 0; max-width: none; max-height: none; min-width: 0; min-height: 0; vertical-align: 0; line-height: normal; text-decoration: none}
img.MathJax_strut {border: 0!important; padding: 0!important; margin: 0!important; vertical-align: 0!important}
.MathJax span {display: inline; position: static; border: 0; padding: 0; margin: 0; vertical-align: 0; line-height: normal; text-decoration: none; box-sizing: content-box}
.MathJax nobr {white-space: nowrap!important}
.MathJax img {display: inline!important; float: none!important}
.MathJax * {transition: none; -webkit-transition: none; -moz-transition: none; -ms-transition: none; -o-transition: none}
.MathJax_Processing {visibility: hidden; position: fixed; width: 0; height: 0; overflow: hidden}
.MathJax_Processed {display: none!important}
.MathJax_ExBox {display: block!important; overflow: hidden; width: 1px; height: 60ex; min-height: 0; max-height: none}
.MathJax .MathJax_EmBox {display: block!important; overflow: hidden; width: 1px; height: 60em; min-height: 0; max-height: none}
.MathJax_LineBox {display: table!important}
.MathJax_LineBox span {display: table-cell!important; width: 10000em!important; min-width: 0; max-width: none; padding: 0; border: 0; margin: 0}
.MathJax .MathJax_HitBox {cursor: text; background: white; opacity: 0; filter: alpha(opacity=0)}
.MathJax .MathJax_HitBox * {filter: none; opacity: 1; background: transparent}
#MathJax_Tooltip * {filter: none; opacity: 1; background: transparent}
@font-face {font-family: MathJax_Blank; src: url('about:blank')}
.MathJax .noError {vertical-align: ; font-size: 90%; text-align: left; color: black; padding: 1px 3px; border: 1px solid}
</style><link type="text/css" rel="stylesheet" href="./README_files/foldgutter.css"><link type="text/css" rel="stylesheet" href="./README_files/foldgutter(1).css"><link type="text/css" rel="stylesheet" href="./README_files/main(3).css"><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/codefolding/firstline-fold" src="./README_files/firstline-fold.js.download"></script><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="nbextensions/codefolding/magic-fold" src="./README_files/magic-fold.js.download"></script><style type="text/css">/*
 * Copyright (c) Jupyter Development Team.
 * Distributed under the terms of the Modified BSD License.
 */

/* Override the correction for the prompt area in https://github.com/jupyter/notebook/blob/dd41d9fd5c4f698bd7468612d877828a7eeb0e7a/IPython/html/static/notebook/less/outputarea.less#L110 */

.jupyter-widgets-output-area div.output_subarea {
    max-width: 100%;
}

/* Work-around for the bug fixed in https://github.com/jupyter/notebook/pull/2961 */

.jupyter-widgets-output-area > .out_prompt_overlay {
    display: none;
}
</style><style type="text/css">/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
.p-Widget {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  position: relative;
  overflow: hidden;
  cursor: default;
}
.p-Widget.p-mod-hidden {
  display: none !important;
}
/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
.p-CommandPalette {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.p-CommandPalette-search {
  -webkit-box-flex: 0;
      -ms-flex: 0 0 auto;
          flex: 0 0 auto;
}
.p-CommandPalette-content {
  -webkit-box-flex: 1;
      -ms-flex: 1 1 auto;
          flex: 1 1 auto;
  margin: 0;
  padding: 0;
  min-height: 0;
  overflow: auto;
  list-style-type: none;
}
.p-CommandPalette-header {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.p-CommandPalette-item {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
}
.p-CommandPalette-itemIcon {
  -webkit-box-flex: 0;
      -ms-flex: 0 0 auto;
          flex: 0 0 auto;
}
.p-CommandPalette-itemContent {
  -webkit-box-flex: 1;
      -ms-flex: 1 1 auto;
          flex: 1 1 auto;
}
.p-CommandPalette-itemShortcut {
  -webkit-box-flex: 0;
      -ms-flex: 0 0 auto;
          flex: 0 0 auto;
}
.p-CommandPalette-itemLabel {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
.p-DockPanel {
  z-index: 0;
}
.p-DockPanel-widget {
  z-index: 0;
}
.p-DockPanel-tabBar {
  z-index: 1;
}
.p-DockPanel-handle {
  z-index: 2;
}
.p-DockPanel-handle.p-mod-hidden {
  display: none !important;
}
.p-DockPanel-handle:after {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: '';
}
.p-DockPanel-handle[data-orientation='horizontal'] {
  cursor: ew-resize;
}
.p-DockPanel-handle[data-orientation='vertical'] {
  cursor: ns-resize;
}
.p-DockPanel-handle[data-orientation='horizontal']:after {
  left: 50%;
  min-width: 8px;
  -webkit-transform: translateX(-50%);
          transform: translateX(-50%);
}
.p-DockPanel-handle[data-orientation='vertical']:after {
  top: 50%;
  min-height: 8px;
  -webkit-transform: translateY(-50%);
          transform: translateY(-50%);
}
.p-DockPanel-overlay {
  z-index: 3;
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  pointer-events: none;
}
.p-DockPanel-overlay.p-mod-hidden {
  display: none !important;
}
/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
.p-Menu {
  z-index: 10000;
  position: absolute;
  white-space: nowrap;
  overflow-x: hidden;
  overflow-y: auto;
  outline: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.p-Menu-content {
  margin: 0;
  padding: 0;
  display: table;
  list-style-type: none;
}
.p-Menu-item {
  display: table-row;
}
.p-Menu-item.p-mod-hidden,
.p-Menu-item.p-mod-collapsed {
  display: none !important;
}
.p-Menu-itemIcon,
.p-Menu-itemSubmenuIcon {
  display: table-cell;
  text-align: center;
}
.p-Menu-itemLabel {
  display: table-cell;
  text-align: left;
}
.p-Menu-itemShortcut {
  display: table-cell;
  text-align: right;
}
/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
.p-MenuBar {
  outline: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.p-MenuBar-content {
  margin: 0;
  padding: 0;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
  list-style-type: none;
}
.p-MenuBar-item {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
}
.p-MenuBar-itemIcon,
.p-MenuBar-itemLabel {
  display: inline-block;
}
/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
.p-ScrollBar {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.p-ScrollBar[data-orientation='horizontal'] {
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
}
.p-ScrollBar[data-orientation='vertical'] {
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
}
.p-ScrollBar-button {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  -webkit-box-flex: 0;
      -ms-flex: 0 0 auto;
          flex: 0 0 auto;
}
.p-ScrollBar-track {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  position: relative;
  overflow: hidden;
  -webkit-box-flex: 1;
      -ms-flex: 1 1 auto;
          flex: 1 1 auto;
}
.p-ScrollBar-thumb {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  position: absolute;
}
/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
.p-SplitPanel-child {
  z-index: 0;
}
.p-SplitPanel-handle {
  z-index: 1;
}
.p-SplitPanel-handle.p-mod-hidden {
  display: none !important;
}
.p-SplitPanel-handle:after {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: '';
}
.p-SplitPanel[data-orientation='horizontal'] > .p-SplitPanel-handle {
  cursor: ew-resize;
}
.p-SplitPanel[data-orientation='vertical'] > .p-SplitPanel-handle {
  cursor: ns-resize;
}
.p-SplitPanel[data-orientation='horizontal'] > .p-SplitPanel-handle:after {
  left: 50%;
  min-width: 8px;
  -webkit-transform: translateX(-50%);
          transform: translateX(-50%);
}
.p-SplitPanel[data-orientation='vertical'] > .p-SplitPanel-handle:after {
  top: 50%;
  min-height: 8px;
  -webkit-transform: translateY(-50%);
          transform: translateY(-50%);
}
/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
.p-TabBar {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.p-TabBar[data-orientation='horizontal'] {
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
}
.p-TabBar[data-orientation='vertical'] {
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
}
.p-TabBar-content {
  margin: 0;
  padding: 0;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
      -ms-flex: 1 1 auto;
          flex: 1 1 auto;
  list-style-type: none;
}
.p-TabBar[data-orientation='horizontal'] > .p-TabBar-content {
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
}
.p-TabBar[data-orientation='vertical'] > .p-TabBar-content {
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
}
.p-TabBar-tab {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  overflow: hidden;
}
.p-TabBar-tabIcon,
.p-TabBar-tabCloseIcon {
  -webkit-box-flex: 0;
      -ms-flex: 0 0 auto;
          flex: 0 0 auto;
}
.p-TabBar-tabLabel {
  -webkit-box-flex: 1;
      -ms-flex: 1 1 auto;
          flex: 1 1 auto;
  overflow: hidden;
  white-space: nowrap;
}
.p-TabBar-tab.p-mod-hidden {
  display: none !important;
}
.p-TabBar.p-mod-dragging .p-TabBar-tab {
  position: relative;
}
.p-TabBar.p-mod-dragging[data-orientation='horizontal'] .p-TabBar-tab {
  left: 0;
  -webkit-transition: left 150ms ease;
  transition: left 150ms ease;
}
.p-TabBar.p-mod-dragging[data-orientation='vertical'] .p-TabBar-tab {
  top: 0;
  -webkit-transition: top 150ms ease;
  transition: top 150ms ease;
}
.p-TabBar.p-mod-dragging .p-TabBar-tab.p-mod-dragging {
  -webkit-transition: none;
  transition: none;
}
/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/
.p-TabPanel-tabBar {
  z-index: 1;
}
.p-TabPanel-stackedPanel {
  z-index: 0;
}
</style><style type="text/css">/* Copyright (c) Jupyter Development Team.
 * Distributed under the terms of the Modified BSD License.
 */

 .jupyter-widgets-disconnected::before {
    content: "\F127"; /* chain-broken */
    display: inline-block;
    font: normal normal normal 14px/1 FontAwesome;
    font-size: inherit;
    text-rendering: auto;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #d9534f;
    padding: 3px;
    -ms-flex-item-align: start;
        align-self: flex-start;
}
</style><style type="text/css">/* Copyright (c) Jupyter Development Team.
 * Distributed under the terms of the Modified BSD License.
 */

 /* We import all of these together in a single css file because the Webpack
loader sees only one file at a time. This allows postcss to see the variable
definitions when they are used. */

 /*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

 /*
This file is copied from the JupyterLab project to define default styling for
when the widget styling is compiled down to eliminate CSS variables. We make one
change - we comment out the font import below.
*/

 /**
 * The material design colors are adapted from google-material-color v1.2.6
 * https://github.com/danlevan/google-material-color
 * https://github.com/danlevan/google-material-color/blob/f67ca5f4028b2f1b34862f64b0ca67323f91b088/dist/palette.var.css
 *
 * The license for the material design color CSS variables is as follows (see
 * https://github.com/danlevan/google-material-color/blob/f67ca5f4028b2f1b34862f64b0ca67323f91b088/LICENSE)
 *
 * The MIT License (MIT)
 *
 * Copyright (c) 2014 Dan Le Van
 *
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the "Software"), to deal
 * in the Software without restriction, including without limitation the rights
 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 * copies of the Software, and to permit persons to whom the Software is
 * furnished to do so, subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in
 * all copies or substantial portions of the Software.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
 * SOFTWARE.
 */

 /*
The following CSS variables define the main, public API for styling JupyterLab.
These variables should be used by all plugins wherever possible. In other
words, plugins should not define custom colors, sizes, etc unless absolutely
necessary. This enables users to change the visual theme of JupyterLab
by changing these variables.

Many variables appear in an ordered sequence (0,1,2,3). These sequences
are designed to work well together, so for example, `--jp-border-color1` should
be used with `--jp-layout-color1`. The numbers have the following meanings:

* 0: super-primary, reserved for special emphasis
* 1: primary, most important under normal situations
* 2: secondary, next most important under normal situations
* 3: tertiary, next most important under normal situations

Throughout JupyterLab, we are mostly following principles from Google's
Material Design when selecting colors. We are not, however, following
all of MD as it is not optimized for dense, information rich UIs.
*/

 /*
 * Optional monospace font for input/output prompt.
 */

 /* Commented out in ipywidgets since we don't need it. */

 /* @import url('https://fonts.googleapis.com/css?family=Roboto+Mono'); */

 /*
 * Added for compabitility with output area
 */

 :root {

  /* Borders

  The following variables, specify the visual styling of borders in JupyterLab.
   */

  /* UI Fonts

  The UI font CSS variables are used for the typography all of the JupyterLab
  user interface elements that are not directly user generated content.
  */ /* Base font size */ /* Ensures px perfect FontAwesome icons */

  /* Use these font colors against the corresponding main layout colors.
     In a light theme, these go from dark to light.
  */

  /* Use these against the brand/accent/warn/error colors.
     These will typically go from light to darker, in both a dark and light theme
   */

  /* Content Fonts

  Content font variables are used for typography of user generated content.
  */ /* Base font size */


  /* Layout

  The following are the main layout colors use in JupyterLab. In a light
  theme these would go from light to dark.
  */

  /* Brand/accent */

  /* State colors (warn, error, success, info) */

  /* Cell specific styles */
  /* A custom blend of MD grey and blue 600
   * See https://meyerweb.com/eric/tools/color-blend/#546E7A:1E88E5:5:hex */
  /* A custom blend of MD grey and orange 600
   * https://meyerweb.com/eric/tools/color-blend/#546E7A:F4511E:5:hex */

  /* Notebook specific styles */

  /* Console specific styles */

  /* Toolbar specific styles */
}

 /* Copyright (c) Jupyter Development Team.
 * Distributed under the terms of the Modified BSD License.
 */

 /*
 * We assume that the CSS variables in
 * https://github.com/jupyterlab/jupyterlab/blob/master/src/default-theme/variables.css
 * have been defined.
 */

 /* This file has code derived from PhosphorJS CSS files, as noted below. The license for this PhosphorJS code is:

Copyright (c) 2014-2017, PhosphorJS Contributors
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

* Neither the name of the copyright holder nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

*/

 /*
 * The following section is derived from https://github.com/phosphorjs/phosphor/blob/23b9d075ebc5b73ab148b6ebfc20af97f85714c4/packages/widgets/style/tabbar.css 
 * We've scoped the rules so that they are consistent with exactly our code.
 */

 .jupyter-widgets.widget-tab > .p-TabBar {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

 .jupyter-widgets.widget-tab > .p-TabBar[data-orientation='horizontal'] {
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
}

 .jupyter-widgets.widget-tab > .p-TabBar[data-orientation='vertical'] {
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
}

 .jupyter-widgets.widget-tab > .p-TabBar > .p-TabBar-content {
  margin: 0;
  padding: 0;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
      -ms-flex: 1 1 auto;
          flex: 1 1 auto;
  list-style-type: none;
}

 .jupyter-widgets.widget-tab > .p-TabBar[data-orientation='horizontal'] > .p-TabBar-content {
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
}

 .jupyter-widgets.widget-tab > .p-TabBar[data-orientation='vertical'] > .p-TabBar-content {
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tab {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  overflow: hidden;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tabIcon,
.jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tabCloseIcon {
  -webkit-box-flex: 0;
      -ms-flex: 0 0 auto;
          flex: 0 0 auto;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tabLabel {
  -webkit-box-flex: 1;
      -ms-flex: 1 1 auto;
          flex: 1 1 auto;
  overflow: hidden;
  white-space: nowrap;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tab.p-mod-hidden {
  display: none !important;
}

 .jupyter-widgets.widget-tab > .p-TabBar.p-mod-dragging .p-TabBar-tab {
  position: relative;
}

 .jupyter-widgets.widget-tab > .p-TabBar.p-mod-dragging[data-orientation='horizontal'] .p-TabBar-tab {
  left: 0;
  -webkit-transition: left 150ms ease;
  transition: left 150ms ease;
}

 .jupyter-widgets.widget-tab > .p-TabBar.p-mod-dragging[data-orientation='vertical'] .p-TabBar-tab {
  top: 0;
  -webkit-transition: top 150ms ease;
  transition: top 150ms ease;
}

 .jupyter-widgets.widget-tab > .p-TabBar.p-mod-dragging .p-TabBar-tab.p-mod-dragging {
  -webkit-transition: none;
  transition: none;
}

 /* End tabbar.css */

 :root { /* margin between inline elements */

    /* From Material Design Lite */
}

 .jupyter-widgets {
    margin: 2px;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    color: black;
    overflow: visible;
}

 .jupyter-widgets.jupyter-widgets-disconnected::before {
    line-height: 28px;
    height: 28px;
}

 .jp-Output-result > .jupyter-widgets {
    margin-left: 0;
    margin-right: 0;
}

 /* vbox and hbox */

 .widget-inline-hbox {
    /* Horizontal widgets */
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: horizontal;
    -webkit-box-direction: normal;
        -ms-flex-direction: row;
            flex-direction: row;
    -webkit-box-align: baseline;
        -ms-flex-align: baseline;
            align-items: baseline;
}

 .widget-inline-vbox {
    /* Vertical Widgets */
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
}

 .widget-box {
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    margin: 0;
    overflow: auto;
}

 .widget-gridbox {
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    display: grid;
    margin: 0;
    overflow: auto;
}

 .widget-hbox {
    -webkit-box-orient: horizontal;
    -webkit-box-direction: normal;
        -ms-flex-direction: row;
            flex-direction: row;
}

 .widget-vbox {
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
}

 /* General Button Styling */

 .jupyter-button {
    padding-left: 10px;
    padding-right: 10px;
    padding-top: 0px;
    padding-bottom: 0px;
    display: inline-block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    text-align: center;
    font-size: 13px;
    cursor: pointer;

    height: 28px;
    border: 0px solid;
    line-height: 28px;
    -webkit-box-shadow: none;
            box-shadow: none;

    color: rgba(0, 0, 0, .8);
    background-color: #EEEEEE;
    border-color: #E0E0E0;
    border: none;
}

 .jupyter-button i.fa {
    margin-right: 4px;
    pointer-events: none;
}

 .jupyter-button:empty:before {
    content: "\200B"; /* zero-width space */
}

 .jupyter-widgets.jupyter-button:disabled {
    opacity: 0.6;
}

 .jupyter-button i.fa.center {
    margin-right: 0;
}

 .jupyter-button:hover:enabled, .jupyter-button:focus:enabled {
    /* MD Lite 2dp shadow */
    -webkit-box-shadow: 0 2px 2px 0 rgba(0, 0, 0, .14),
                0 3px 1px -2px rgba(0, 0, 0, .2),
                0 1px 5px 0 rgba(0, 0, 0, .12);
            box-shadow: 0 2px 2px 0 rgba(0, 0, 0, .14),
                0 3px 1px -2px rgba(0, 0, 0, .2),
                0 1px 5px 0 rgba(0, 0, 0, .12);
}

 .jupyter-button:active, .jupyter-button.mod-active {
    /* MD Lite 4dp shadow */
    -webkit-box-shadow: 0 4px 5px 0 rgba(0, 0, 0, .14),
                0 1px 10px 0 rgba(0, 0, 0, .12),
                0 2px 4px -1px rgba(0, 0, 0, .2);
            box-shadow: 0 4px 5px 0 rgba(0, 0, 0, .14),
                0 1px 10px 0 rgba(0, 0, 0, .12),
                0 2px 4px -1px rgba(0, 0, 0, .2);
    color: rgba(0, 0, 0, .8);
    background-color: #BDBDBD;
}

 .jupyter-button:focus:enabled {
    outline: 1px solid #64B5F6;
}

 /* Button "Primary" Styling */

 .jupyter-button.mod-primary {
    color: rgba(255, 255, 255, 1.0);
    background-color: #2196F3;
}

 .jupyter-button.mod-primary.mod-active {
    color: rgba(255, 255, 255, 1);
    background-color: #1976D2;
}

 .jupyter-button.mod-primary:active {
    color: rgba(255, 255, 255, 1);
    background-color: #1976D2;
}

 /* Button "Success" Styling */

 .jupyter-button.mod-success {
    color: rgba(255, 255, 255, 1.0);
    background-color: #4CAF50;
}

 .jupyter-button.mod-success.mod-active {
    color: rgba(255, 255, 255, 1);
    background-color: #388E3C;
 }

 .jupyter-button.mod-success:active {
    color: rgba(255, 255, 255, 1);
    background-color: #388E3C;
 }

 /* Button "Info" Styling */

 .jupyter-button.mod-info {
    color: rgba(255, 255, 255, 1.0);
    background-color: #00BCD4;
}

 .jupyter-button.mod-info.mod-active {
    color: rgba(255, 255, 255, 1);
    background-color: #0097A7;
}

 .jupyter-button.mod-info:active {
    color: rgba(255, 255, 255, 1);
    background-color: #0097A7;
}

 /* Button "Warning" Styling */

 .jupyter-button.mod-warning {
    color: rgba(255, 255, 255, 1.0);
    background-color: #FF9800;
}

 .jupyter-button.mod-warning.mod-active {
    color: rgba(255, 255, 255, 1);
    background-color: #F57C00;
}

 .jupyter-button.mod-warning:active {
    color: rgba(255, 255, 255, 1);
    background-color: #F57C00;
}

 /* Button "Danger" Styling */

 .jupyter-button.mod-danger {
    color: rgba(255, 255, 255, 1.0);
    background-color: #F44336;
}

 .jupyter-button.mod-danger.mod-active {
    color: rgba(255, 255, 255, 1);
    background-color: #D32F2F;
}

 .jupyter-button.mod-danger:active {
    color: rgba(255, 255, 255, 1);
    background-color: #D32F2F;
}

 /* Widget Button*/

 .widget-button, .widget-toggle-button {
    width: 148px;
}

 /* Widget Label Styling */

 /* Override Bootstrap label css */

 .jupyter-widgets label {
    margin-bottom: 0;
    margin-bottom: initial;
}

 .widget-label-basic {
    /* Basic Label */
    color: black;
    font-size: 13px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    line-height: 28px;
}

 .widget-label {
    /* Label */
    color: black;
    font-size: 13px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    line-height: 28px;
}

 .widget-inline-hbox .widget-label {
    /* Horizontal Widget Label */
    color: black;
    text-align: right;
    margin-right: 8px;
    width: 80px;
    -ms-flex-negative: 0;
        flex-shrink: 0;
}

 .widget-inline-vbox .widget-label {
    /* Vertical Widget Label */
    color: black;
    text-align: center;
    line-height: 28px;
}

 /* Widget Readout Styling */

 .widget-readout {
    color: black;
    font-size: 13px;
    height: 28px;
    line-height: 28px;
    overflow: hidden;
    white-space: nowrap;
    text-align: center;
}

 .widget-readout.overflow {
    /* Overflowing Readout */

    /* From Material Design Lite
        shadow-key-umbra-opacity: 0.2;
        shadow-key-penumbra-opacity: 0.14;
        shadow-ambient-shadow-opacity: 0.12;
     */
    -webkit-box-shadow: 0 2px 2px 0 rgba(0, 0, 0, .2),
                        0 3px 1px -2px rgba(0, 0, 0, .14),
                        0 1px 5px 0 rgba(0, 0, 0, .12);

    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, .2),
                0 3px 1px -2px rgba(0, 0, 0, .14),
                0 1px 5px 0 rgba(0, 0, 0, .12);
}

 .widget-inline-hbox .widget-readout {
    /* Horizontal Readout */
    text-align: center;
    max-width: 148px;
    min-width: 72px;
    margin-left: 4px;
}

 .widget-inline-vbox .widget-readout {
    /* Vertical Readout */
    margin-top: 4px;
    /* as wide as the widget */
    width: inherit;
}

 /* Widget Checkbox Styling */

 .widget-checkbox {
    width: 300px;
    height: 28px;
    line-height: 28px;
}

 .widget-checkbox input[type="checkbox"] {
    margin: 0px 8px 0px 0px;
    line-height: 28px;
    font-size: large;
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    -ms-flex-negative: 0;
        flex-shrink: 0;
    -ms-flex-item-align: center;
        align-self: center;
}

 /* Widget Valid Styling */

 .widget-valid {
    height: 28px;
    line-height: 28px;
    width: 148px;
    font-size: 13px;
}

 .widget-valid i:before {
    line-height: 28px;
    margin-right: 4px;
    margin-left: 4px;

    /* from the fa class in FontAwesome: https://github.com/FortAwesome/Font-Awesome/blob/49100c7c3a7b58d50baa71efef11af41a66b03d3/css/font-awesome.css#L14 */
    display: inline-block;
    font: normal normal normal 14px/1 FontAwesome;
    font-size: inherit;
    text-rendering: auto;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

 .widget-valid.mod-valid i:before {
    content: "\F00C";
    color: green;
}

 .widget-valid.mod-invalid i:before {
    content: "\F00D";
    color: red;
}

 .widget-valid.mod-valid .widget-valid-readout {
    display: none;
}

 /* Widget Text and TextArea Stying */

 .widget-textarea, .widget-text {
    width: 300px;
}

 .widget-text input[type="text"], .widget-text input[type="number"]{
    height: 28px;
    line-height: 28px;
}

 .widget-text input[type="text"]:disabled, .widget-text input[type="number"]:disabled, .widget-textarea textarea:disabled {
    opacity: 0.6;
}

 .widget-text input[type="text"], .widget-text input[type="number"], .widget-textarea textarea {
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    border: 1px solid #9E9E9E;
    background-color: white;
    color: rgba(0, 0, 0, .8);
    font-size: 13px;
    padding: 4px 8px;
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    min-width: 0; /* This makes it possible for the flexbox to shrink this input */
    -ms-flex-negative: 1;
        flex-shrink: 1;
    outline: none !important;
}

 .widget-textarea textarea {
    height: inherit;
    width: inherit;
}

 .widget-text input:focus, .widget-textarea textarea:focus {
    border-color: #64B5F6;
}

 /* Widget Slider */

 .widget-slider .ui-slider {
    /* Slider Track */
    border: 1px solid #BDBDBD;
    background: #BDBDBD;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    position: relative;
    border-radius: 0px;
}

 .widget-slider .ui-slider .ui-slider-handle {
    /* Slider Handle */
    outline: none !important; /* focused slider handles are colored - see below */
    position: absolute;
    background-color: white;
    border: 1px solid #9E9E9E;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    z-index: 1;
    background-image: none; /* Override jquery-ui */
}

 /* Override jquery-ui */

 .widget-slider .ui-slider .ui-slider-handle:hover, .widget-slider .ui-slider .ui-slider-handle:focus {
    background-color: #2196F3;
    border: 1px solid #2196F3;
}

 .widget-slider .ui-slider .ui-slider-handle:active {
    background-color: #2196F3;
    border-color: #2196F3;
    z-index: 2;
    -webkit-transform: scale(1.2);
            transform: scale(1.2);
}

 .widget-slider  .ui-slider .ui-slider-range {
    /* Interval between the two specified value of a double slider */
    position: absolute;
    background: #2196F3;
    z-index: 0;
}

 /* Shapes of Slider Handles */

 .widget-hslider .ui-slider .ui-slider-handle {
    width: 16px;
    height: 16px;
    margin-top: -7px;
    margin-left: -7px;
    border-radius: 50%;
    top: 0;
}

 .widget-vslider .ui-slider .ui-slider-handle {
    width: 16px;
    height: 16px;
    margin-bottom: -7px;
    margin-left: -7px;
    border-radius: 50%;
    left: 0;
}

 .widget-hslider .ui-slider .ui-slider-range {
    height: 8px;
    margin-top: -3px;
}

 .widget-vslider .ui-slider .ui-slider-range {
    width: 8px;
    margin-left: -3px;
}

 /* Horizontal Slider */

 .widget-hslider {
    width: 300px;
    height: 28px;
    line-height: 28px;

    /* Override the align-items baseline. This way, the description and readout
    still seem to align their baseline properly, and we don't have to have
    align-self: stretch in the .slider-container. */
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
}

 .widgets-slider .slider-container {
    overflow: visible;
}

 .widget-hslider .slider-container {
    height: 28px;
    margin-left: 6px;
    margin-right: 6px;
    -webkit-box-flex: 1;
        -ms-flex: 1 1 148px;
            flex: 1 1 148px;
}

 .widget-hslider .ui-slider {
    /* Inner, invisible slide div */
    height: 4px;
    margin-top: 12px;
    width: 100%;
}

 /* Vertical Slider */

 .widget-vbox .widget-label {
    height: 28px;
    line-height: 28px;
}

 .widget-vslider {
    /* Vertical Slider */
    height: 200px;
    width: 72px;
}

 .widget-vslider .slider-container {
    -webkit-box-flex: 1;
        -ms-flex: 1 1 148px;
            flex: 1 1 148px;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 6px;
    margin-top: 6px;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
}

 .widget-vslider .ui-slider-vertical {
    /* Inner, invisible slide div */
    width: 4px;
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    margin-left: auto;
    margin-right: auto;
}

 /* Widget Progress Styling */

 .progress-bar {
    -webkit-transition: none;
    transition: none;
}

 .progress-bar {
    height: 28px;
}

 .progress-bar {
    background-color: #2196F3;
}

 .progress-bar-success {
    background-color: #4CAF50;
}

 .progress-bar-info {
    background-color: #00BCD4;
}

 .progress-bar-warning {
    background-color: #FF9800;
}

 .progress-bar-danger {
    background-color: #F44336;
}

 .progress {
    background-color: #EEEEEE;
    border: none;
    -webkit-box-shadow: none;
            box-shadow: none;
}

 /* Horisontal Progress */

 .widget-hprogress {
    /* Progress Bar */
    height: 28px;
    line-height: 28px;
    width: 300px;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;

}

 .widget-hprogress .progress {
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    margin-top: 4px;
    margin-bottom: 4px;
    -ms-flex-item-align: stretch;
        align-self: stretch;
    /* Override bootstrap style */
    height: auto;
    height: initial;
}

 /* Vertical Progress */

 .widget-vprogress {
    height: 200px;
    width: 72px;
}

 .widget-vprogress .progress {
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    width: 20px;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 0;
}

 /* Select Widget Styling */

 .widget-dropdown {
    height: 28px;
    width: 300px;
    line-height: 28px;
}

 .widget-dropdown > select {
    padding-right: 20px;
    border: 1px solid #9E9E9E;
    border-radius: 0;
    height: inherit;
    -webkit-box-flex: 1;
        -ms-flex: 1 1 148px;
            flex: 1 1 148px;
    min-width: 0; /* This makes it possible for the flexbox to shrink this input */
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    outline: none !important;
    -webkit-box-shadow: none;
            box-shadow: none;
    background-color: white;
    color: rgba(0, 0, 0, .8);
    font-size: 13px;
    vertical-align: top;
    padding-left: 8px;
	appearance: none;
	-webkit-appearance: none;
	-moz-appearance: none;
    background-repeat: no-repeat;
	background-size: 20px;
	background-position: right center;
    background-image: url("data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDE5LjIuMSwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMCkgIC0tPgo8c3ZnIHZlcnNpb249IjEuMSIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4IgoJIHZpZXdCb3g9IjAgMCAxOCAxOCIgc3R5bGU9ImVuYWJsZS1iYWNrZ3JvdW5kOm5ldyAwIDAgMTggMTg7IiB4bWw6c3BhY2U9InByZXNlcnZlIj4KPHN0eWxlIHR5cGU9InRleHQvY3NzIj4KCS5zdDB7ZmlsbDpub25lO30KPC9zdHlsZT4KPHBhdGggZD0iTTUuMiw1LjlMOSw5LjdsMy44LTMuOGwxLjIsMS4ybC00LjksNWwtNC45LTVMNS4yLDUuOXoiLz4KPHBhdGggY2xhc3M9InN0MCIgZD0iTTAtMC42aDE4djE4SDBWLTAuNnoiLz4KPC9zdmc+Cg");
}

 .widget-dropdown > select:focus {
    border-color: #64B5F6;
}

 .widget-dropdown > select:disabled {
    opacity: 0.6;
}

 /* To disable the dotted border in Firefox around select controls.
   See http://stackoverflow.com/a/18853002 */

 .widget-dropdown > select:-moz-focusring {
    color: transparent;
    text-shadow: 0 0 0 #000;
}

 /* Select and SelectMultiple */

 .widget-select {
    width: 300px;
    line-height: 28px;

    /* Because Firefox defines the baseline of a select as the bottom of the
    control, we align the entire control to the top and add padding to the
    select to get an approximate first line baseline alignment. */
    -webkit-box-align: start;
        -ms-flex-align: start;
            align-items: flex-start;
}

 .widget-select > select {
    border: 1px solid #9E9E9E;
    background-color: white;
    color: rgba(0, 0, 0, .8);
    font-size: 13px;
    -webkit-box-flex: 1;
        -ms-flex: 1 1 148px;
            flex: 1 1 148px;
    outline: none !important;
    overflow: auto;
    height: inherit;

    /* Because Firefox defines the baseline of a select as the bottom of the
    control, we align the entire control to the top and add padding to the
    select to get an approximate first line baseline alignment. */
    padding-top: 5px;
}

 .widget-select > select:focus {
    border-color: #64B5F6;
}

 .wiget-select > select > option {
    padding-left: 4px;
    line-height: 28px;
    /* line-height doesn't work on some browsers for select options */
    padding-top: calc(28px - var(--jp-widgets-font-size) / 2);
    padding-bottom: calc(28px - var(--jp-widgets-font-size) / 2);
}

 /* Toggle Buttons Styling */

 .widget-toggle-buttons {
    line-height: 28px;
}

 .widget-toggle-buttons .widget-toggle-button {
    margin-left: 2px;
    margin-right: 2px;
}

 .widget-toggle-buttons .jupyter-button:disabled {
    opacity: 0.6;
}

 /* Radio Buttons Styling */

 .widget-radio {
    width: 300px;
    line-height: 28px;
}

 .widget-radio-box {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-align: stretch;
        -ms-flex-align: stretch;
            align-items: stretch;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    margin-bottom: 8px;
}

 .widget-radio-box label {
    height: 20px;
    line-height: 20px;
    font-size: 13px;
}

 .widget-radio-box input {
    height: 20px;
    line-height: 20px;
    margin: 0 8px 0 1px;
    float: left;
}

 /* Color Picker Styling */

 .widget-colorpicker {
    width: 300px;
    height: 28px;
    line-height: 28px;
}

 .widget-colorpicker > .widget-colorpicker-input {
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    -ms-flex-negative: 1;
        flex-shrink: 1;
    min-width: 72px;
}

 .widget-colorpicker input[type="color"] {
    width: 28px;
    height: 28px;
    padding: 0 2px; /* make the color square actually square on Chrome on OS X */
    background: white;
    color: rgba(0, 0, 0, .8);
    border: 1px solid #9E9E9E;
    border-left: none;
    -webkit-box-flex: 0;
        -ms-flex-positive: 0;
            flex-grow: 0;
    -ms-flex-negative: 0;
        flex-shrink: 0;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    -ms-flex-item-align: stretch;
        align-self: stretch;
    outline: none !important;
}

 .widget-colorpicker.concise input[type="color"] {
    border-left: 1px solid #9E9E9E;
}

 .widget-colorpicker input[type="color"]:focus, .widget-colorpicker input[type="text"]:focus {
    border-color: #64B5F6;
}

 .widget-colorpicker input[type="text"] {
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    outline: none !important;
    height: 28px;
    line-height: 28px;
    background: white;
    color: rgba(0, 0, 0, .8);
    border: 1px solid #9E9E9E;
    font-size: 13px;
    padding: 4px 8px;
    min-width: 0; /* This makes it possible for the flexbox to shrink this input */
    -ms-flex-negative: 1;
        flex-shrink: 1;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
}

 .widget-colorpicker input[type="text"]:disabled {
    opacity: 0.6;
}

 /* Date Picker Styling */

 .widget-datepicker {
    width: 300px;
    height: 28px;
    line-height: 28px;
}

 .widget-datepicker input[type="date"] {
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    -ms-flex-negative: 1;
        flex-shrink: 1;
    min-width: 0; /* This makes it possible for the flexbox to shrink this input */
    outline: none !important;
    height: 28px;
    border: 1px solid #9E9E9E;
    background-color: white;
    color: rgba(0, 0, 0, .8);
    font-size: 13px;
    padding: 4px 8px;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
}

 .widget-datepicker input[type="date"]:focus {
    border-color: #64B5F6;
}

 .widget-datepicker input[type="date"]:invalid {
    border-color: #FF9800;
}

 .widget-datepicker input[type="date"]:disabled {
    opacity: 0.6;
}

 /* Play Widget */

 .widget-play {
    width: 148px;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: stretch;
        -ms-flex-align: stretch;
            align-items: stretch;
}

 .widget-play .jupyter-button {
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    height: auto;
}

 .widget-play .jupyter-button:disabled {
    opacity: 0.6;
}

 /* Tab Widget */

 .jupyter-widgets.widget-tab {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
}

 .jupyter-widgets.widget-tab > .p-TabBar {
    /* Necessary so that a tab can be shifted down to overlay the border of the box below. */
    overflow-x: visible;
    overflow-y: visible;
}

 .jupyter-widgets.widget-tab > .p-TabBar > .p-TabBar-content {
    /* Make sure that the tab grows from bottom up */
    -webkit-box-align: end;
        -ms-flex-align: end;
            align-items: flex-end;
    min-width: 0;
    min-height: 0;
}

 .jupyter-widgets.widget-tab > .widget-tab-contents {
    width: 100%;
    -webkit-box-sizing: border-box;
            box-sizing: border-box;
    margin: 0;
    background: white;
    color: rgba(0, 0, 0, .8);
    border: 1px solid #9E9E9E;
    padding: 15px;
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    overflow: auto;
}

 .jupyter-widgets.widget-tab > .p-TabBar {
    font: 13px Helvetica, Arial, sans-serif;
    min-height: 25px;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tab {
    -webkit-box-flex: 0;
        -ms-flex: 0 1 144px;
            flex: 0 1 144px;
    min-width: 35px;
    min-height: 25px;
    line-height: 24px;
    margin-left: -1px;
    padding: 0px 10px;
    background: #EEEEEE;
    color: rgba(0, 0, 0, .5);
    border: 1px solid #9E9E9E;
    border-bottom: none;
    position: relative;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tab.p-mod-current {
    color: rgba(0, 0, 0, 1.0);
    /* We want the background to match the tab content background */
    background: white;
    min-height: 26px;
    -webkit-transform: translateY(1px);
            transform: translateY(1px);
    overflow: visible;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tab.p-mod-current:before {
    position: absolute;
    top: -1px;
    left: -1px;
    content: '';
    height: 2px;
    width: calc(100% + 2px);
    background: #2196F3;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tab:first-child {
    margin-left: 0;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tab:hover:not(.p-mod-current) {
    background: white;
    color: rgba(0, 0, 0, .8);
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-mod-closable > .p-TabBar-tabCloseIcon {
    margin-left: 4px;
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-mod-closable > .p-TabBar-tabCloseIcon:before {
    font-family: FontAwesome;
    content: '\F00D'; /* close */
}

 .jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tabIcon,
.jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tabLabel,
.jupyter-widgets.widget-tab > .p-TabBar .p-TabBar-tabCloseIcon {
    line-height: 24px;
}

 /* Accordion Widget */

 .p-Collapse {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-align: stretch;
        -ms-flex-align: stretch;
            align-items: stretch;
}

 .p-Collapse-header {
    padding: 4px;
    cursor: pointer;
    color: rgba(0, 0, 0, .5);
    background-color: #EEEEEE;
    border: 1px solid #9E9E9E;
    padding: 10px 15px;
    font-weight: bold;
}

 .p-Collapse-header:hover {
    background-color: white;
    color: rgba(0, 0, 0, .8);
}

 .p-Collapse-open > .p-Collapse-header {
    background-color: white;
    color: rgba(0, 0, 0, 1.0);
    cursor: default;
    border-bottom: none;
}

 .p-Collapse .p-Collapse-header::before {
    content: '\F0DA\A0';  /* caret-right, non-breaking space */
    display: inline-block;
    font: normal normal normal 14px/1 FontAwesome;
    font-size: inherit;
    text-rendering: auto;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

 .p-Collapse-open > .p-Collapse-header::before {
    content: '\F0D7\A0'; /* caret-down, non-breaking space */
}

 .p-Collapse-contents {
    padding: 15px;
    background-color: white;
    color: rgba(0, 0, 0, .8);
    border-left: 1px solid #9E9E9E;
    border-right: 1px solid #9E9E9E;
    border-bottom: 1px solid #9E9E9E;
    overflow: auto;
}

 .p-Accordion {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
    -webkit-box-align: stretch;
        -ms-flex-align: stretch;
            align-items: stretch;
}

 .p-Accordion .p-Collapse {
    margin-bottom: 0;
}

 .p-Accordion .p-Collapse + .p-Collapse {
    margin-top: 4px;
}

 /* HTML widget */

 .widget-html, .widget-htmlmath {
    font-size: 13px;
}

 .widget-html > .widget-html-content, .widget-htmlmath > .widget-html-content {
    /* Fill out the area in the HTML widget */
    -ms-flex-item-align: stretch;
        align-self: stretch;
    -webkit-box-flex: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    -ms-flex-negative: 1;
        flex-shrink: 1;
    /* Makes sure the baseline is still aligned with other elements */
    line-height: 28px;
    /* Make it possible to have absolutely-positioned elements in the html */
    position: relative;
}
</style><link id="favicon" type="image/x-icon" rel="shortcut icon" href="http://localhost:8888/static/base/images/favicon-notebook.ico"><style type="text/css">@font-face {font-family: STIXMathJax_Main-italic; src: url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/STIX-Web/woff/STIXMathJax_Main-Italic.woff?V=2.7.4') format('woff'), url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/STIX-Web/otf/STIXMathJax_Main-Italic.otf?V=2.7.4') format('opentype')}
</style><style type="text/css">@font-face {font-family: STIXMathJax_Main; src: url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/STIX-Web/woff/STIXMathJax_Main-Regular.woff?V=2.7.4') format('woff'), url('http://localhost:8888/static/components/MathJax/fonts/HTML-CSS/STIX-Web/otf/STIXMathJax_Main-Regular.otf?V=2.7.4') format('opentype')}
</style></head>

<body class="notebook_app command_mode" data-jupyter-api-token="a2c36e0b154b3be5250fc47ff193e33d307676b80998c6a5" data-base-url="/" data-ws-url="" data-notebook-name="Untitled8.ipynb" data-notebook-path="Untitled8.ipynb" dir="ltr" style=""><div style="visibility: hidden; overflow: hidden; position: absolute; top: 0px; height: 1px; width: auto; padding: 0px; border: 0px; margin: 0px; text-align: left; text-indent: 0px; text-transform: none; line-height: normal; letter-spacing: normal; word-spacing: normal;"><div id="MathJax_Hidden"><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br></div></div><div id="MathJax_Message" style="display: none;"></div>

<noscript>
    <div id='noscript'>
      Jupyter Notebook requires JavaScript.<br>
      Please enable it to proceed. 
  </div>
</noscript>

<div id="header" style="display: block;">
  <div id="header-container" class="container">
  <div id="ipython_notebook" class="nav navbar-brand"><a href="http://localhost:8888/tree?token=a2c36e0b154b3be5250fc47ff193e33d307676b80998c6a5" title="dashboard">
      <img src="./README_files/logo.png" alt="Jupyter Notebook">
  </a></div>

  


<span id="save_widget" class="save_widget">
    <span id="notebook_name" class="filename">Mammographic_Masses_Dataset</span>
    <span class="checkpoint_status" title="ven. 22 févr. 2019 15:16">Last Checkpoint: il y a 2 heures</span>
    <span class="autosave_status">(autosaved)</span>
<i id="notebook-trusted-indicator" class="fa notebook-trusted fa-check" title="Notebook is trusted"></i></span>

<span id="kernel_logo_widget">
  
  <img class="current_kernel_logo" alt="Current Kernel Logo" src="./README_files/logo-64x64.png" title="Python 3" style="display: inline;">
  
</span>


  
  
  
  

    <span id="login_widget">
      
        <button id="logout" class="btn btn-sm navbar-btn">Logout</button>
      
    </span>

  

  
  
  </div>
  <div class="header-bar"></div>

  
<div id="menubar-container" class="container">
<div id="menubar">
    <div id="menus" class="navbar navbar-default" role="navigation">
        <div class="container-fluid">
            <button type="button" class="btn btn-default navbar-btn navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
              <i class="fa fa-bars"></i>
              <span class="navbar-text">Menu</span>
            </button>
            <p id="kernel_indicator" class="navbar-text indicator_area">
              <span class="kernel_indicator_name">Python 3</span>
              <i id="kernel_indicator_icon" class="kernel_idle_icon" title="Kernel Idle"></i>
            </p>
            <i id="readonly-indicator" class="navbar-text" title="This notebook is read-only" style="display: none;">
                <span class="fa-stack">
                    <i class="fa fa-save fa-stack-1x"></i>
                    <i class="fa fa-ban fa-stack-2x text-danger"></i>
                </span>
            </i>
            <i id="modal_indicator" class="navbar-text modal_indicator" title="Command Mode"></i>
            <span id="notification_area"><div id="notification_kernel" class="notification_widget btn btn-xs navbar-btn undefined info" style="display: none;"><span></span></div><div id="notification_notebook" class="notification_widget btn btn-xs navbar-btn" style="display: none;"><span></span></div><div id="notification_trusted" class="notification_widget btn btn-xs navbar-btn" disabled="disabled" style="cursor: help;"><span title="Javascript enabled for notebook display">Trusted</span></div><div id="notification_widgets" class="notification_widget btn btn-xs navbar-btn" style="display: none;"><span></span></div></span>
            <div class="navbar-collapse collapse">
              <ul class="nav navbar-nav">
                <li class="dropdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="dropdown-toggle" data-toggle="dropdown">File</a>
                    <ul id="file_menu" class="dropdown-menu">
                        <li id="new_notebook" class="dropdown-submenu">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">New Notebook</a>
                            <ul class="dropdown-menu" id="menu-new-notebook-submenu"><li id="new-notebook-submenu-python3"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Python 3</a></li></ul>
                        </li>
                        <li id="open_notebook" title="Opens a new window with the Dashboard view">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Open...</a></li>
                        <!-- <hr/> -->
                        <li class="divider"></li>
                        <li id="copy_notebook" title="Open a copy of this notebook&#39;s contents and start a new kernel">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Make a Copy...</a></li>
                        <li id="save_notebook_as" title="Save a copy of the notebook&#39;s contents and start a new kernel">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Save as...</a></li>
                        <li id="rename_notebook"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Rename...</a></li>
                        <li id="save_checkpoint"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Save and Checkpoint</a></li>
                        <!-- <hr/> -->
                        <li class="divider"></li>
                        <li id="restore_checkpoint" class="dropdown-submenu"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Revert to Checkpoint</a>
                          <ul class="dropdown-menu"><li><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">vendredi 22 février 2019 15:16</a></li></ul>
                        </li>
                        <li class="divider"></li>
                        <li id="print_preview"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Print Preview</a></li>
                        <li class="dropdown-submenu"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Download as</a>
                            <ul id="download_menu" class="dropdown-menu">
                                <li id="download_ipynb"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Notebook (.ipynb)</a></li>
                                <li id="download_script"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Python (.py)</a></li>
                                <li id="download_html"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">HTML (.html)</a></li>
                                <li id="download_slides"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Reveal.js slides (.html)</a></li>
                                <li id="download_markdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Markdown (.md)</a></li>
                                <li id="download_rst"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">reST (.rst)</a></li>
                                <li id="download_latex"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">LaTeX (.tex)</a></li>
                                <li id="download_pdf"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">PDF via LaTeX (.pdf)</a></li>
                                
                                <li id="download_asciidoc">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">asciidoc (.asciidoc)</a>
                                </li>
                                
                                <li id="download_custom">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">custom (.txt)</a>
                                </li>
                                
                                <li id="download_html">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">custom (.html)</a>
                                </li>
                                
                                <li id="download_html_ch">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">custom (.html)</a>
                                </li>
                                
                                <li id="download_html_embed">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">custom (.html)</a>
                                </li>
                                
                                <li id="download_html_toc">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">custom (.html)</a>
                                </li>
                                
                                <li id="download_html_with_lenvs">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">custom (.html)</a>
                                </li>
                                
                                <li id="download_html_with_toclenvs">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">custom (.html)</a>
                                </li>
                                
                                <li id="download_latex">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">latex (.tex)</a>
                                </li>
                                
                                <li id="download_latex_with_lenvs">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">latex (.tex)</a>
                                </li>
                                
                                <li id="download_markdown">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">markdown (.md)</a>
                                </li>
                                
                                <li id="download_notebook">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">notebook (.ipynb)</a>
                                </li>
                                
                                <li id="download_pdf">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">pdf (.tex)</a>
                                </li>
                                
                                <li id="download_python">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">python (.py)</a>
                                </li>
                                
                                <li id="download_rst">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">rst (.rst)</a>
                                </li>
                                
                                <li id="download_script">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">custom (.txt)</a>
                                </li>
                                
                                <li id="download_selectLanguage">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">notebook (.ipynb)</a>
                                </li>
                                
                                <li id="download_slides">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">slides (.slides.html)</a>
                                </li>
                                
                                <li id="download_slides_with_lenvs">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">slides (.slides.html)</a>
                                </li>
                                
                            <li id="download_html_embed"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">HTML Embedded (.html)</a></li></ul>
                        </li>
                        <li class="dropdown-submenu hidden"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Deploy as</a>
                            <ul id="deploy_menu" class="dropdown-menu"></ul>
                        </li>
                        <li class="divider"></li>
                        <li id="trust_notebook" title="Trust the output of this notebook" class="disabled">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Trusted Notebook</a></li>
                        <li class="divider"></li>
                        <li id="close_and_halt" title="Shutdown this notebook&#39;s kernel, and close this window">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Close and Halt</a></li>
                    </ul>
                </li>
                <li class="dropdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="dropdown-toggle" data-toggle="dropdown">Edit</a>
                    <ul id="edit_menu" class="dropdown-menu">
                        <li id="cut_cell"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Cut Cells</a></li>
                        <li id="copy_cell"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Copy Cells</a></li>
                        <li id="paste_cell_above" class=""><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Paste Cells Above</a></li>
                        <li id="paste_cell_below" class=""><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Paste Cells Below</a></li>
                        <li id="paste_cell_replace" class=""><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Paste Cells &amp; Replace</a></li>
                        <li id="delete_cell"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Delete Cells</a></li>
                        <li id="undelete_cell" class=""><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Undo Delete Cells</a></li>
                        <li class="divider"></li>
                        <li id="split_cell"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Split Cell</a></li>
                        <li id="merge_cell_above"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Merge Cell Above</a></li>
                        <li id="merge_cell_below"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Merge Cell Below</a></li>
                        <li class="divider"></li>
                        <li id="move_cell_up"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Move Cell Up</a></li>
                        <li id="move_cell_down"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Move Cell Down</a></li>
                        <li class="divider"></li>
                        <li id="edit_nb_metadata"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Edit Notebook Metadata</a></li>
                        <li class="divider"></li>
                        <li id="find_and_replace"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#"> Find and Replace </a></li>
                        <li class="divider"></li>
                        <li id="cut_cell_attachments"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Cut Cell Attachments</a></li>
                        <li id="copy_cell_attachments"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Copy Cell Attachments</a></li>
                        <li id="paste_cell_attachments" class="disabled"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Paste Cell Attachments</a></li>
                        <li class="divider"></li>
                        <li id="insert_image" class="disabled"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#"> Insert Image </a></li>
                    <li class="divider"></li><li><a target="_blank" title="Opens in a new window" href="http://localhost:8888/nbextensions/"> <i class="fa fa-cogs menu-icon pull-right"></i><span>nbextensions config</span></a></li></ul>
                </li>
                <li class="dropdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="dropdown-toggle" data-toggle="dropdown">View</a>
                    <ul id="view_menu" class="dropdown-menu">
                        <li id="toggle_header" title="Show/Hide the logo and notebook title (above menu bar)">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Toggle Header</a>
                        </li>
                        <li id="toggle_toolbar" title="Show/Hide the action icons (below menu bar)">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Toggle Toolbar</a>
                        </li>
                        <li id="toggle_line_numbers" title="Show/Hide line numbers in cells">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Toggle Line Numbers</a>
                        </li>
                        <li id="menu-cell-toolbar" class="dropdown-submenu">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Cell Toolbar</a>
                            <ul class="dropdown-menu" id="menu-cell-toolbar-submenu"><li data-name="None"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">None</a></li><li data-name="Edit%20Metadata"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Edit Metadata</a></li><li data-name="Raw%20Cell%20Format"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Raw Cell Format</a></li><li data-name="Slideshow"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Slideshow</a></li><li data-name="Attachments"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Attachments</a></li><li data-name="Tags"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Tags</a></li></ul>
                        </li>
                    </ul>
                </li>
                <li class="dropdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="dropdown-toggle" data-toggle="dropdown" aria-expanded="false">Insert</a>
                    <ul id="insert_menu" class="dropdown-menu">
                        <li id="insert_cell_above" title="Insert an empty Code cell above the currently active cell">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Insert Cell Above</a></li>
                        <li id="insert_cell_below" title="Insert an empty Code cell below the currently active cell">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Insert Cell Below</a></li>
                    <li class="divider"></li><li><a title="Insert a heading cell above the selected cell" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Insert Heading Above</a></li><li><a title="Insert a heading cell below the selected cell&#39;s section" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Insert Heading Below</a></li></ul>
                </li>
                <li class="dropdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="dropdown-toggle" data-toggle="dropdown" aria-expanded="false">Cell</a>
                    <ul id="cell_menu" class="dropdown-menu">
                        <li id="run_cell" title="Run this cell, and move cursor to the next one">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Run Cells</a></li>
                        <li id="run_cell_select_below" title="Run this cell, select below">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Run Cells and Select Below</a></li>
                        <li id="run_cell_insert_below" title="Run this cell, insert below">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Run Cells and Insert Below</a></li>
                        <li id="run_all_cells" title="Run all cells in the notebook">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Run All</a></li>
                        <li id="run_all_cells_above" title="Run all cells above (but not including) this cell">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Run All Above</a></li>
                        <li id="run_all_cells_below" title="Run this cell and all cells below it">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Run All Below</a></li>
                        <li class="divider"></li>
                        <li id="change_cell_type" class="dropdown-submenu" title="All cells in the notebook have a cell type. By default, new cells are created as &#39;Code&#39; cells">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Cell Type</a>
                            <ul class="dropdown-menu">
                              <li id="to_code" title="Contents will be sent to the kernel for execution, and output will display in the footer of cell">
                                  <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Code</a></li>
                              <li id="to_markdown" title="Contents will be rendered as HTML and serve as explanatory text">
                                  <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Markdown</a></li>
                              <li id="to_raw" title="Contents will pass through nbconvert unmodified">
                                  <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Raw NBConvert</a></li>
                            </ul>
                        </li>
                        <li class="divider"></li>
                        <li id="current_outputs" class="dropdown-submenu"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Current Outputs</a>
                            <ul class="dropdown-menu">
                                <li id="toggle_current_output" title="Hide/Show the output of the current cell">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Toggle</a>
                                </li>
                                <li id="toggle_current_output_scroll" title="Scroll the output of the current cell">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Toggle Scrolling</a>
                                </li>
                                <li id="clear_current_output" title="Clear the output of the current cell">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Clear</a>
                                </li>
                            </ul>
                        </li>
                        <li id="all_outputs" class="dropdown-submenu"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">All Output</a>
                            <ul class="dropdown-menu">
                                <li id="toggle_all_output" title="Hide/Show the output of all cells">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Toggle</a>
                                </li>
                                <li id="toggle_all_output_scroll" title="Scroll the output of all cells">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Toggle Scrolling</a>
                                </li>
                                <li id="clear_all_output" title="Clear the output of all cells">
                                    <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Clear</a>
                                </li>
                            </ul>
                        </li>
                    </ul>
                </li>
                <li class="dropdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="dropdown-toggle" data-toggle="dropdown">Kernel</a>
                    <ul id="kernel_menu" class="dropdown-menu">
                        <li id="int_kernel" title="Send Keyboard Interrupt (CTRL-C) to the Kernel">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Interrupt</a>
                        </li>
                        <li id="restart_kernel" title="Restart the Kernel">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Restart</a>
                        </li>
                        <li id="restart_clear_output" title="Restart the Kernel and clear all output">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Restart &amp; Clear Output</a>
                        </li>
                        <li id="restart_run_all" title="Restart the Kernel and re-run the notebook">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Restart &amp; Run All</a>
                        </li>
                        <li id="reconnect_kernel" title="Reconnect to the Kernel">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Reconnect</a>
                        </li>
                        <li id="shutdown_kernel" title="Shutdown the Kernel">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Shutdown</a>
                        </li>
                        <li class="divider"></li>
                        <li id="menu-change-kernel" class="dropdown-submenu">
                            <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Change kernel</a>
                            <ul class="dropdown-menu" id="menu-change-kernel-submenu"><li id="kernel-submenu-python3"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Python 3</a></li></ul>
                        </li>
                    </ul>
                </li><li id="Navigate" class="dropdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" id="Navigate_sub" class="dropdown-toggle" data-toggle="dropdown">Navigate</a><ul id="Navigate_menu" class="dropdown-menu ui-resizable" style=""><div id="navigate_menu" class="toc" style="width: 160px; height: 0px;"><ul class="toc-item"><li><span><i class="fa fa-fw fa-caret-down"></i><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Overview" data-toc-modified-id="Overview-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Overview</a></span><ul class="toc-item"><li><ul class="toc-item"><li><span><i class="fa fa-fw"></i><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Reading-the-data-file-in-df" data-toc-modified-id="Reading-the-data-file-in-df-1.0.1"><span class="toc-item-num">1.0.1&nbsp;&nbsp;</span>Reading the data file in df</a></span></li><li><span><i class="fa fa-fw"></i><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Decision-Tree" data-toc-modified-id="Decision-Tree-1.0.2"><span class="toc-item-num">1.0.2&nbsp;&nbsp;</span>Decision Tree</a></span></li><li><span><i class="fa fa-fw"></i><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Random-Forests" data-toc-modified-id="Random-Forests-1.0.3" class="toc-item-highlight-select"><span class="toc-item-num">1.0.3&nbsp;&nbsp;</span>Random Forests</a></span></li></ul></li></ul></li></ul></div><div class="ui-resizable-handle ui-resizable-e" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-s" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-se ui-icon ui-icon-gripsmall-diagonal-se" style="z-index: 90;"></div></ul></li>
                <li class="dropdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" data-toggle="dropdown" class="dropdown-toggle">Widgets</a><ul id="widget-submenu" class="dropdown-menu"><li title="Save the notebook with the widget state information for static rendering"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Save Notebook Widget State</a></li><li title="Clear the widget state information from the notebook"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Clear Notebook Widget State</a></li><ul class="divider"></ul><li title="Download the widget state as a JSON file"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Download Widget State</a></li><li title="Embed interactive widgets"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Embed Widgets</a></li></ul></li><li class="dropdown"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="dropdown-toggle" data-toggle="dropdown" aria-expanded="false">Help</a>
                    <ul id="help_menu" class="dropdown-menu">
                        
                        <li id="notebook_tour" title="A quick tour of the notebook user interface"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">User Interface Tour</a></li>
                        <li id="keyboard_shortcuts" title="Opens a tooltip with all keyboard shortcuts"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Keyboard Shortcuts</a></li>
                        <li id="edit_keyboard_shortcuts" title="Opens a dialog allowing you to edit Keyboard shortcuts"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">Edit Keyboard Shortcuts</a></li>
                        <li class="divider"></li>
                        

						
                        
                            
                                <li><a rel="noreferrer" href="http://nbviewer.jupyter.org/github/ipython/ipython/blob/3.x/examples/Notebook/Index.ipynb" target="_blank" title="Opens in a new window">
                                
                                    <i class="fa fa-external-link menu-icon pull-right"></i>
                                

                                Notebook Help
                                </a></li>
                            
                                <li><a rel="noreferrer" href="https://help.github.com/articles/markdown-basics/" target="_blank" title="Opens in a new window">
                                
                                    <i class="fa fa-external-link menu-icon pull-right"></i>
                                

                                Markdown
                                </a></li>
                            
                            
                        
                        <li><a title="Jupyter_contrib_nbextensions documentation" id="jupyter_contrib_nbextensions_help" href="http://jupyter-contrib-nbextensions.readthedocs.io/en/latest/" target="_blank">Jupyter-contrib <br> nbextensions<i class="fa fa-external-link menu-icon pull-right"></i></a></li><li id="kernel-help-links" class="divider"></li><li><a target="_blank" title="Opens in a new window" href="https://docs.python.org/3.7?v=20190222144558"><i class="fa fa-external-link menu-icon pull-right"></i><span>Python Reference</span></a></li><li><a target="_blank" title="Opens in a new window" href="https://ipython.org/documentation.html?v=20190222144558"><i class="fa fa-external-link menu-icon pull-right"></i><span>IPython Reference</span></a></li><li><a target="_blank" title="Opens in a new window" href="https://docs.scipy.org/doc/numpy/reference/?v=20190222144558"><i class="fa fa-external-link menu-icon pull-right"></i><span>NumPy Reference</span></a></li><li><a target="_blank" title="Opens in a new window" href="https://docs.scipy.org/doc/scipy/reference/?v=20190222144558"><i class="fa fa-external-link menu-icon pull-right"></i><span>SciPy Reference</span></a></li><li><a target="_blank" title="Opens in a new window" href="https://matplotlib.org/contents.html?v=20190222144558"><i class="fa fa-external-link menu-icon pull-right"></i><span>Matplotlib Reference</span></a></li><li><a target="_blank" title="Opens in a new window" href="http://docs.sympy.org/latest/index.html?v=20190222144558"><i class="fa fa-external-link menu-icon pull-right"></i><span>SymPy Reference</span></a></li><li><a target="_blank" title="Opens in a new window" href="https://pandas.pydata.org/pandas-docs/stable/?v=20190222144558"><i class="fa fa-external-link menu-icon pull-right"></i><span>pandas Reference</span></a></li><li class="divider"></li>
                        <li title="About Jupyter Notebook"><a id="notebook_about" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#">About</a></li>
                        
                    </ul>
                </li>
              </ul>
            </div>
        </div>
    </div>
</div>

<div id="maintoolbar" class="navbar">
  <div class="toolbar-inner navbar-inner navbar-nobg">
    <div id="maintoolbar-container" class="container toolbar"><div class="btn-group" id="save-notbook"><button class="btn btn-default" title="Save and Checkpoint" data-jupyter-action="jupyter-notebook:save-notebook"><i class="fa-save fa"></i></button></div><div class="btn-group" id="insert_above_below"><button class="btn btn-default" title="insert cell below" data-jupyter-action="jupyter-notebook:insert-cell-below"><i class="fa-plus fa"></i></button></div><div class="btn-group" id="cut_copy_paste"><button class="btn btn-default" title="cut selected cells" data-jupyter-action="jupyter-notebook:cut-cell"><i class="fa-cut fa"></i></button><button class="btn btn-default" title="copy selected cells" data-jupyter-action="jupyter-notebook:copy-cell"><i class="fa-copy fa"></i></button><button class="btn btn-default" title="paste cells below" data-jupyter-action="jupyter-notebook:paste-cell-below"><i class="fa-paste fa"></i></button></div><div class="btn-group" id="move_up_down"><button class="btn btn-default" title="move selected cells up" data-jupyter-action="jupyter-notebook:move-cell-up"><i class="fa-arrow-up fa"></i></button><button class="btn btn-default" title="move selected cells down" data-jupyter-action="jupyter-notebook:move-cell-down"><i class="fa-arrow-down fa"></i></button></div><div class="btn-group" id="run_int"><button class="btn btn-default" title="Run" data-jupyter-action="jupyter-notebook:run-cell-and-select-next"><i class="fa-step-forward fa"></i><span class="toolbar-btn-label">Run</span></button><button class="btn btn-default" title="interrupt the kernel" data-jupyter-action="jupyter-notebook:interrupt-kernel"><i class="fa-stop fa"></i></button><button class="btn btn-default" title="restart the kernel (with dialog)" data-jupyter-action="jupyter-notebook:confirm-restart-kernel"><i class="fa-repeat fa"></i></button><button class="btn btn-default" title="restart the kernel, then re-run the whole notebook (with dialog)" data-jupyter-action="jupyter-notebook:confirm-restart-kernel-and-run-all-cells"><i class="fa-forward fa"></i></button></div><select id="cell_type" class="form-control select-xs"><option value="code">Code</option><option value="markdown">Markdown</option><option value="raw">Raw NBConvert</option><option value="heading">Heading</option><option value="multiselect" disabled="disabled" style="display: none;">-</option></select><div class="btn-group"><button class="btn btn-default" title="open the command palette" data-jupyter-action="jupyter-notebook:show-command-palette"><i class="fa-keyboard-o fa"></i></button></div><div class="btn-group"><button class="btn btn-default" title="Variable Inspector" data-jupyter-action="varInspector:toggle-variable-inspector" id="varInspector_button"><i class="fa-crosshairs fa"></i></button></div><div class="btn-group"><button class="btn btn-default" title="Increase code font size" data-jupyter-action="code_font_size:increase-code-font-size"><i class="fa-search-plus fa"></i></button><button class="btn btn-default" title="Decrease code font size" data-jupyter-action="code_font_size:decrease-code-font-size"><i class="fa-search-minus fa"></i></button></div><div class="btn-group"><button class="btn btn-default" title="Create/Edit Gist of Notebook" data-jupyter-action="gist_it:create-gist-from-notebook"><i class="fa-github fa"></i></button></div><div class="btn-group" id="code_prettify_button"><button class="btn btn-default" title="code_prettify selected cell(s) (add shift for all cells)"><i class="fa-legal fa"></i></button></div><div class="btn-group" id="autopep8_button"><button class="btn btn-default" title="autopep8 selected cell(s) (add shift for all cells)"><i class="fa-legal fa"></i></button></div><div class="btn-group"><button class="btn btn-default" title="Table of Contents" data-jupyter-action="toc2:toggle-toc" id="toc_button"><i class="fa-list fa"></i></button></div></div>
  </div>
</div>
</div>

<div class="lower-header-bar"></div>

</div>

<div id="site" style="display: block; height: 603.444px;"><div id="toc-wrapper" style="display: none; position: relative; width: 20%; top: 163.333px; left: 0px;" class="ui-draggable ui-resizable sidebar-wrapper"><div id="toc-header"><span class="header">Contents </span><i class="fa fa-fw hide-btn" title="Hide ToC"></i><i class="fa fa-fw fa-refresh" title="Reload ToC"></i><i class="fa fa-fw fa-cog" title="ToC settings"></i></div><div id="toc" class="toc"><ul class="toc-item"><li><span class="highlight_on_scroll"><i class="fa fa-fw fa-caret-down"></i><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Overview" data-toc-modified-id="Overview-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Overview</a></span><ul class="toc-item"><li><ul class="toc-item"><li><span class=""><i class="fa fa-fw"></i><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Reading-the-data-file-in-df" data-toc-modified-id="Reading-the-data-file-in-df-1.0.1"><span class="toc-item-num">1.0.1&nbsp;&nbsp;</span>Reading the data file in df</a></span></li><li><span class=""><i class="fa fa-fw"></i><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Decision-Tree" data-toc-modified-id="Decision-Tree-1.0.2"><span class="toc-item-num">1.0.2&nbsp;&nbsp;</span>Decision Tree</a></span></li><li><span class=""><i class="fa fa-fw"></i><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Random-Forests" data-toc-modified-id="Random-Forests-1.0.3" class="toc-item-highlight-select"><span class="toc-item-num">1.0.3&nbsp;&nbsp;</span>Random Forests</a></span></li></ul></li></ul></li></ul></div><div class="ui-resizable-handle ui-resizable-n" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-e ui-icon ui-icon-grip-dotted-vertical" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-s" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-w" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-se ui-icon-gripsmall-diagonal-se" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-sw" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-ne" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-nw" style="z-index: 90;"></div></div>


<div id="ipython-main-app">
    <div id="notebook_panel">
        <div id="notebook" tabindex="-1"><div class="container" id="notebook-container"><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h1"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 176.861px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1" draggable="false"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 33px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 142.861px; top: 0px; height: 21.3333px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-1"># Overview</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 33px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 44px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h1 id="Overview" data-toc-modified-id="Overview-1"><a class="toc-mod-link" id="Overview-1"></a><span class="toc-item-num">1&nbsp;&nbsp;</span>Overview<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Overview">¶</a></h1>
</div></div></div><div class="cell text_cell unrendered unselected" tabindex="2"><div class="prompt input_prompt"></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 39.3333px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true" style="bottom: 0px;"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1" draggable="false"><div class="CodeMirror-sizer" style="margin-left: 34px; padding-right: 0px; padding-bottom: 0px; margin-bottom: -19px; border-right-width: 11px; min-height: 501px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>17</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 5.33334px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation" style=""><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">We are going to work with a dataset on Mammography. </span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">The original dataset is avaiable at UCI Machine Learning Repository.</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">3</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span cm-text="">​</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">4</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">Mammography is the most effective method for breast cancer screening available today. However, the low positive predictive value of breast biopsy resulting from mammogram interpretation leads to approximately 70% unnecessary biopsies with benign outcomes. To reduce the high number of unnecessary breast biopsies, several computer-aided diagnosis (CAD) systems have been proposed in the last years. These systems help physicians in their decision to perform a breast biopsy on a suspicious lesion seen in a mammogram or to perform a short term follow-up examination instead.</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">5</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span cm-text="">​</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">6</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">The dataset has the following 6 attributes including 1 target/goal attribute: </span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">7</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span cm-text="">​</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">8</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">Attribute Information:</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">9</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">-BI-RADS assessment: 1 to 5 (ordinal, non-predictive!)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">10</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">-Age: patient's age in years (integer)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">11</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">-Shape: mass shape: round=1 oval=2 lobular=3 irregular=4 (nominal)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">12</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">-Margin: mass margin: circumscribed=1 microlobulated=2 obscured=3 ill-defined=4 spiculated=5 (nominal)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">13</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">-Density: mass density high=1 iso=2 low=3 fat-containing=4 (ordinal)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">14</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">-Severity: benign=0 or malignant=1 (binominal, goal field!) -- we named it as Target in the cleaned version of the data set.</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">15</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span cm-text="">​</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">16</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span cm-text="">​</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">17</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">The csv file provied is a cleaner version of the original dataset, the dataset is treated with missing values. Althoug, you can download the original dataset and do some cleaning.</span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 501px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 512px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"></div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[1]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 73.1527px; left: 190px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 247.181px; margin-bottom: -19px; border-right-width: 11px; min-height: 96px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 144px; top: 67.5556px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation" style=""><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-keyword">import</span> <span class="cm-variable">pandas</span> <span class="cm-keyword">as</span> <span class="cm-variable">pd</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-keyword">import</span> <span class="cm-variable">numpy</span> <span class="cm-keyword">as</span> <span class="cm-variable">np</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">3</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-keyword">import</span> <span class="cm-variable">matplotlib</span>.<span class="cm-property">pyplot</span> <span class="cm-keyword">as</span> <span class="cm-variable">plt</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">4</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-keyword">import</span> <span class="cm-variable">seaborn</span> <span class="cm-keyword">as</span> <span class="cm-variable">sns</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">5</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-operator">%</span><span class="cm-variable">matplotlib</span> <span class="cm-variable">inline</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 96px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 107px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h3"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 293.556px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 30px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 259.556px; top: 0px; height: 18.6667px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-3">### Reading the data file in df</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 30px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 41px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h3 id="Reading-the-data-file-in-df" data-toc-modified-id="Reading-the-data-file-in-df-1.0.1"><a class="toc-mod-link" id="Reading-the-data-file-in-df-1.0.1"></a><span class="toc-item-num">1.0.1&nbsp;&nbsp;</span>Reading the data file in df<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Reading-the-data-file-in-df">¶</a></h3>
</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 386.833px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 352.833px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's read the csv data file and disply its head</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-read-the-csv-data-file-and-disply-its-head">Let's read the csv data file and disply its head<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-read-the-csv-data-file-and-disply-its-head">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[2]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 22.4861px; left: 120.847px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 424.208px; margin-bottom: -19px; border-right-width: 11px; min-height: 45px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>2</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 74.8472px; top: 16.8889px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">df</span> <span class="cm-operator">=</span> <span class="cm-variable">pd</span>.<span class="cm-property">read_csv</span>(<span class="cm-string">'mammographic_masses_data_clean.csv'</span>)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">df</span>.<span class="cm-property">head</span><span class=" CodeMirror-matchingbracket">(</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 45px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 56px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt output_prompt"><bdi>Out[2]:</bdi></div><div class="output_subarea output_html rendered_html output_result"><div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BI-RADS</th>
      <th>Age</th>
      <th>Shape</th>
      <th>Margin</th>
      <th>Density</th>
      <th>Target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>67</td>
      <td>3</td>
      <td>5</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>58</td>
      <td>4</td>
      <td>5</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>28</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5</td>
      <td>57</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>76</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 344.819px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 310.819px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's see how many data entries we got</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-see-how-many-data-entries-we-got">Let's see how many data entries we got<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-see-how-many-data-entries-we-got">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[3]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 113.153px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 77.8472px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><pre><span>xxxxxxxxxx</span></pre></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 67.1528px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">df</span>.<span class="cm-property">info</span><span class=" CodeMirror-matchingbracket">(</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt"></div><div class="output_subarea output_text output_stream output_stdout"><pre>&lt;class 'pandas.core.frame.DataFrame'&gt;
RangeIndex: 830 entries, 0 to 829
Data columns (total 6 columns):
BI-RADS    830 non-null int64
Age        830 non-null int64
Shape      830 non-null int64
Margin     830 non-null int64
Density    830 non-null int64
Target     830 non-null int64
dtypes: int64(6)
memory usage: 39.0 KB
</pre></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 337.056px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 303.056px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's see what type cancer is common</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-see-what-type-cancer-is-common">Let's see what type cancer is common<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-see-what-type-cancer-is-common">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[4]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 251.694px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 216.389px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><pre><span>xxxxxxxxxx</span></pre></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 205.694px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">df</span>[<span class="cm-string">'Target'</span>].<span class="cm-property">value_counts</span><span class=" CodeMirror-matchingbracket">(</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt output_prompt"><bdi>Out[4]:</bdi></div><div class="output_subarea output_text output_result"><pre>0    427
1    403
Name: Target, dtype: int64</pre></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 225.778px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 191.778px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's decribe the data</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-decribe-the-data">Let's decribe the data<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-decribe-the-data">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[5]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 143.944px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 108.639px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 97.9445px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">df</span>.<span class="cm-property">describe</span><span class=" CodeMirror-matchingbracket">(</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt output_prompt"><bdi>Out[5]:</bdi></div><div class="output_subarea output_html rendered_html output_result"><div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BI-RADS</th>
      <th>Age</th>
      <th>Shape</th>
      <th>Margin</th>
      <th>Density</th>
      <th>Target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>830.000000</td>
      <td>830.000000</td>
      <td>830.000000</td>
      <td>830.000000</td>
      <td>830.000000</td>
      <td>830.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>4.393976</td>
      <td>55.781928</td>
      <td>2.781928</td>
      <td>2.813253</td>
      <td>2.915663</td>
      <td>0.485542</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.888371</td>
      <td>14.671782</td>
      <td>1.242361</td>
      <td>1.567175</td>
      <td>0.350936</td>
      <td>0.500092</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>18.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>4.000000</td>
      <td>46.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>4.000000</td>
      <td>57.000000</td>
      <td>3.000000</td>
      <td>3.000000</td>
      <td>3.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>5.000000</td>
      <td>66.000000</td>
      <td>4.000000</td>
      <td>4.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>55.000000</td>
      <td>96.000000</td>
      <td>4.000000</td>
      <td>5.000000</td>
      <td>4.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 312.903px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><pre><span>xxxxxxxxxx</span></pre></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 278.903px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's plot the common cancer type</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-plot-the-common-cancer-type">Let's plot the common cancer type<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-plot-the-common-cancer-type">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[7]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 22.4861px; left: 120.847px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 454.556px; margin-bottom: -19px; border-right-width: 11px; min-height: 45px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>2</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 74.8472px; top: 16.8889px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">sns</span>.<span class="cm-property">countplot</span>(<span class="cm-variable">x</span><span class="cm-operator">=</span><span class="cm-string">'Target'</span>, <span class="cm-variable">palette</span> <span class="cm-operator">=</span><span class="cm-string">'coolwarm'</span>, <span class="cm-variable">data</span> <span class="cm-operator">=</span> <span class="cm-variable">df</span> )</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">plt</span>.<span class="cm-property">show</span><span class=" CodeMirror-matchingbracket">(</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 45px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 56px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt"></div><div class="output_subarea output_png"><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAYgAAAEKCAYAAAAIO8L1AAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDMuMC4xLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvDW2N/gAAEoVJREFUeJzt3X+sX3d93/HnCycEujDy64amtpkj6q0NqJjo4kWLVLEEtSFbcdoRlqwtHsvkbgobrNXW0E00bI3USqVpYW1Uo4Q4qCNYUBqXZXSpE0ZRS8INmBAnzXAhJbf24gv50WS0qWze++P78fLF+dj3a+Nzvze5z4f01fecz/mcc99XuvLLn/M5P1JVSJJ0uBdNuwBJ0vJkQEiSugwISVKXASFJ6jIgJEldBoQkqcuAkCR1GRCSpC4DQpLUddK0C/hunHXWWbVu3bpplyFJzyv33nvvN6pqZrF+z+uAWLduHXNzc9MuQ5KeV5L8+ST9PMUkSeoyICRJXQaEJKnLgJAkdRkQkqQuA0KS1GVASJK6DAhJUpcBIUnqel7fSX0i3P75p6ddgpahS19/6rRLkKbOEYQkqcuAkCR1GRCSpC4DQpLUZUBIkroMCElSlwEhSeoyICRJXYMHRJJVSb6Y5JNt/dwkdyf5SpKPJnlxaz+lre9p29cNXZsk6ciWYgTxTuDBsfVfAa6vqvXA48BVrf0q4PGq+n7g+tZPkjQlgz5qI8ka4B8B1wE/myTARcA/a122AdcCNwCb2jLAx4D/miRVVUPWKC1X39x567RL0DJ05sVXLNnPGnoE8evAfwC+3dbPBJ6oqgNtfR5Y3ZZXA48AtO1Ptv6SpCkYLCCS/GNgf1XdO97c6VoTbBs/7pYkc0nmFhYWTkClkqSeIUcQFwJvTvIwcCujU0u/DpyW5NCprTXA3rY8D6wFaNtfDjx2+EGramtVzVbV7MzMzIDlS9LKNlhAVNW7q2pNVa0DrgDurKqfBO4C3tK6bQZua8s72jpt+53OP0jS9EzjPoifZzRhvYfRHMONrf1G4MzW/rPANVOoTZLULMkLg6rq08Cn2/JXgY2dPn8NXL4U9UiSFued1JKkLgNCktRlQEiSugwISVKXASFJ6jIgJEldBoQkqcuAkCR1GRCSpC4DQpLUZUBIkroMCElSlwEhSeoyICRJXQaEJKlryHdSvyTJPUm+lGR3kve29puTfC3JrvbZ0NqT5P1J9iS5L8n5Q9UmSVrckC8Mega4qKqeTnIy8Nkk/6Nt+/dV9bHD+r8JWN8+fx+4oX1LkqZgyHdSV1U93VZPbp+jvWN6E3BL2+9zwGlJzhmqPknS0Q06B5FkVZJdwH7gjqq6u226rp1Guj7JKa1tNfDI2O7zrU2SNAWDBkRVHayqDcAaYGOS1wDvBn4AeD1wBvDzrXt6hzi8IcmWJHNJ5hYWFgaqXJK0JFcxVdUTwKeBS6pqXzuN9AzwIWBj6zYPrB3bbQ2wt3OsrVU1W1WzMzMzA1cuSSvXkFcxzSQ5rS2/FHgj8KeH5hWSBLgMuL/tsgN4W7ua6QLgyaraN1R9kqSjG/IqpnOAbUlWMQqi7VX1ySR3JplhdEppF/CvWv/bgUuBPcC3gLcPWJskaRGDBURV3Qe8rtN+0RH6F3D1UPVIko6Nd1JLkroMCElSlwEhSeoyICRJXQaEJKnLgJAkdRkQkqQuA0KS1GVASJK6DAhJUpcBIUnqMiAkSV0GhCSpy4CQJHUZEJKkLgNCktQ15CtHX5LkniRfSrI7yXtb+7lJ7k7ylSQfTfLi1n5KW9/Ttq8bqjZJ0uKGHEE8A1xUVa8FNgCXtHdN/wpwfVWtBx4Hrmr9rwIer6rvB65v/SRJUzJYQNTI02315PYp4CLgY619G3BZW97U1mnbL06SoeqTJB3doHMQSVYl2QXsB+4A/gx4oqoOtC7zwOq2vBp4BKBtfxI4s3PMLUnmkswtLCwMWb4krWiDBkRVHayqDcAaYCPwg71u7bs3WqjnNFRtrarZqpqdmZk5ccVKkr7DklzFVFVPAJ8GLgBOS3JS27QG2NuW54G1AG37y4HHlqI+SdJzDXkV00yS09ryS4E3Ag8CdwFvad02A7e15R1tnbb9zqp6zghCkrQ0Tlq8y3E7B9iWZBWjINpeVZ9M8gBwa5JfAr4I3Nj63wh8OMkeRiOHKwasTZK0iMECoqruA17Xaf8qo/mIw9v/Grh8qHokScfGO6klSV0GhCSpy4CQJHUZEJKkLgNCktRlQEiSugwISVKXASFJ6jIgJEldBoQkqcuAkCR1GRCSpC4DQpLUZUBIkroMCElS15BvlFub5K4kDybZneSdrf3aJH+RZFf7XDq2z7uT7EnyUJIfHao2SdLihnyj3AHg56rqC0leBtyb5I627fqq+tXxzknOY/QWuVcD3wf8YZK/W1UHB6xRknQEg40gqmpfVX2hLT/F6H3Uq4+yyybg1qp6pqq+Buyh8+Y5SdLSWJI5iCTrGL1+9O7W9I4k9yW5KcnprW018MjYbvMcPVAkSQOaKCCS7Jyk7Qj7ngp8HHhXVf0lcAPwKmADsA9436Gund2rc7wtSeaSzC0sLExSgiTpOBw1IJK8JMkZwFlJTk9yRvusYzRPcFRJTmYUDr9TVb8LUFWPVtXBqvo28EGePY00D6wd230NsPfwY1bV1qqararZmZmZxX9DSdJxWWwE8TPAvcAPtO9Dn9uA3zzajkkC3Ag8WFW/NtZ+zli3Hwfub8s7gCuSnJLkXGA9cM/kv4ok6UQ66lVMVfUbwG8k+TdV9YFjPPaFwE8DX06yq7X9AnBlkg2MTh89zCiEqKrdSbYDDzC6Aupqr2CSpOmZ6DLXqvpAkn8ArBvfp6puOco+n6U/r3D7Ufa5DrhukpokScOaKCCSfJjRxPIu4ND/6gs4YkBIkp7fJr1RbhY4r6qec1WRJOmFadL7IO4HvnfIQiRJy8ukI4izgAeS3AM8c6ixqt48SFWSpKmbNCCuHbIISdLyM+lVTP9r6EIkScvLpFcxPcWzj714MXAy8H+r6m8PVZgkabomHUG8bHw9yWX4pFVJekE7rqe5VtXvARed4FokScvIpKeYfmJs9UWM7ovwnghJegGb9CqmHxtbPsDoGUqbTng1kqRlY9I5iLcPXYgkaXmZ9IVBa5J8Isn+JI8m+XiSNUMXJ0mankknqT/E6H0N38foNaC/39okSS9QkwbETFV9qKoOtM/NgK9zk6QXsEkD4htJfirJqvb5KeCbR9shydokdyV5MMnuJO9s7WckuSPJV9r36a09Sd6fZE+S+5Kc/939apKk78akAfEvgLcC/wfYB7wFWGzi+gDwc1X1g8AFwNVJzgOuAXZW1XpgZ1sHeBOj14yuB7YANxzD7yFJOsEmDYj/AmyuqpmqOptRYFx7tB2qal9VfaEtPwU8yGj+YhOwrXXbBlzWljcBt9TI54DTDnt/tSRpCU0aED9UVY8fWqmqx4DXTfpDkqxr/e8GXlFV+9px9gFnt26rgUfGdptvbZKkKZg0IF50aK4ARvMITH4X9qnAx4F3VdVfHq1rp+05d2sn2ZJkLsncwsLCJCVIko7DpHdSvw/44yQfY/SP9luB6xbbKcnJjMLhd6rqd1vzo0nOqap97RTS/tY+D6wd230NsPfwY1bVVmArwOzsrI/7kKSBTDSCqKpbgH8CPAosAD9RVR8+2j5JAtwIPFhVvza2aQewuS1vBm4ba39bu5rpAuDJQ6eiJElLb9IRBFX1APDAMRz7QuCngS8n2dXafgH4ZWB7kquArwOXt223A5cCe4BvsfhVUpKkAU0cEMeqqj5Lf14B4OJO/wKuHqoeSdKxOa73QUiSXvgMCElSlwEhSeoyICRJXQaEJKnLgJAkdRkQkqQuA0KS1GVASJK6DAhJUpcBIUnqMiAkSV0GhCSpy4CQJHUZEJKkrsECIslNSfYnuX+s7dokf5FkV/tcOrbt3Un2JHkoyY8OVZckaTJDjiBuBi7ptF9fVRva53aAJOcBVwCvbvv8VpJVA9YmSVrEYAFRVZ8BHpuw+ybg1qp6pqq+xui1oxuHqk2StLhpzEG8I8l97RTU6a1tNfDIWJ/51iZJmpKlDogbgFcBG4B9wPtae+/d1dU7QJItSeaSzC0sLAxTpSRpaQOiqh6tqoNV9W3ggzx7GmkeWDvWdQ2w9wjH2FpVs1U1OzMzM2zBkrSCLWlAJDlnbPXHgUNXOO0ArkhySpJzgfXAPUtZmyTpO5001IGTfAR4A3BWknngF4E3JNnA6PTRw8DPAFTV7iTbgQeAA8DVVXVwqNokSYsbLCCq6spO841H6X8dcN1Q9UiSjo13UkuSugwISVKXASFJ6jIgJEldBoQkqcuAkCR1GRCSpC4DQpLUZUBIkroMCElSlwEhSeoyICRJXQaEJKnLgJAkdRkQkqQuA0KS1DVYQCS5Kcn+JPePtZ2R5I4kX2nfp7f2JHl/kj1J7kty/lB1SZImM+QI4mbgksPargF2VtV6YGdbB3gTo/dQrwe2ADcMWJckaQKDBURVfQZ47LDmTcC2trwNuGys/ZYa+RxwWpJzhqpNkrS4pZ6DeEVV7QNo32e39tXAI2P95lvbcyTZkmQuydzCwsKgxUrSSrZcJqnTaatex6raWlWzVTU7MzMzcFmStHItdUA8eujUUfve39rngbVj/dYAe5e4NknSmKUOiB3A5ra8GbhtrP1t7WqmC4AnD52KkiRNx0lDHTjJR4A3AGclmQd+EfhlYHuSq4CvA5e37rcDlwJ7gG8Bbx+qLknSZAYLiKq68gibLu70LeDqoWqRJB275TJJLUlaZgwISVKXASFJ6jIgJEldBoQkqcuAkCR1GRCSpC4DQpLUZUBIkroMCElSlwEhSeoyICRJXQaEJKnLgJAkdRkQkqSuwd4HcTRJHgaeAg4CB6pqNskZwEeBdcDDwFur6vFp1CdJmu4I4h9W1Yaqmm3r1wA7q2o9sLOtS5KmZDmdYtoEbGvL24DLpliLJK140wqIAv5nknuTbGltr6iqfQDt++wp1SZJYkpzEMCFVbU3ydnAHUn+dNIdW6BsAXjlK185VH2StOJNZQRRVXvb937gE8BG4NEk5wC07/1H2HdrVc1W1ezMzMxSlSxJK86SB0SSv5XkZYeWgR8B7gd2AJtbt83AbUtdmyTpWdM4xfQK4BNJDv38/1ZVn0ryeWB7kquArwOXT6E2SVKz5AFRVV8FXttp/yZw8VLXI0nqW06XuUqSlhEDQpLUZUBIkroMCElSlwEhSeoyICRJXQaEJKnLgJAkdRkQkqQuA0KS1GVASJK6DAhJUpcBIUnqMiAkSV0GhCSpa9kFRJJLkjyUZE+Sa6ZdjyStVMsqIJKsAn4TeBNwHnBlkvOmW5UkrUzLKiCAjcCeqvpqVf0NcCuwaco1SdKKtNwCYjXwyNj6fGuTJC2xJX8n9SLSaavv6JBsAba01aeTPDR4VSvHWcA3pl2E1OHf5v935Yk4yN+ZpNNyC4h5YO3Y+hpg73iHqtoKbF3KolaKJHNVNTvtOqTD+bc5HcvtFNPngfVJzk3yYuAKYMeUa5KkFWlZjSCq6kCSdwB/AKwCbqqq3VMuS5JWpGUVEABVdTtw+7TrWKE8daflyr/NKUhVLd5LkrTiLLc5CEnSMmFAyMebaNlKclOS/Unun3YtK5EBscL5eBMtczcDl0y7iJXKgJCPN9GyVVWfAR6bdh0rlQEhH28iqcuA0KKPN5G0MhkQWvTxJpJWJgNCPt5EUpcBscJV1QHg0ONNHgS2+3gTLRdJPgL8CfD3kswnuWraNa0k3kktSepyBCFJ6jIgJEldBoQkqcuAkCR1GRCSpK5l98IgaTlIciaws61+L3AQWGjrG9tzq070zzwfOLuqPnWijy0dDwNC6qiqbwIbAJJcCzxdVb866f5JVlXVwWP8secDrwEMCC0LnmKSjlGS309yb5LdSf5lazspyRNJfinJPcDGJG9u79n4oyQfSPJ7re+pSW5Ock+SLyb5sSQvBd4D/GSSXUneMsVfUQIcQUjHY3NVPZbke4C5JB8HngJeDnyhqv5T2/a/gQuBrwPbx/Z/D/CpqvrnSU4H7gZ+CPjPwGuq6l1L+ctIR+IIQjp2/y7Jlxg9AmIN8KrW/jfAJ9ryecBDVfXnNXpcwUfG9v8R4D8m2QXcBbwEeOWSVC4dA0cQ0jFI8kbgh4ELquqvknyW0T/wAH9Vzz67pvcYdca2XVZVf3bYsX/4hBcsfRccQUjH5uXAYy0cXg28/gj9djN6wNzaJAH+6di2PwD+7aGVJK9ri08BLxugZum4GBDSsfnvwPe0U0zvYTR/8BxV9S1GT8n9Q+CPGL1j48m2+b3tGF9Oshu4trXfCby2TVw7Sa2p82mu0kCSnFpVT7cRxG8DX66qD0y7LmlSjiCk4fzrNhH9APBS4INTrkc6Jo4gJEldjiAkSV0GhCSpy4CQJHUZEJKkLgNCktRlQEiSuv4fWzDnu7cWLLAAAAAASUVORK5CYII="></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 39.3333px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 5.33333px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's visualize the biopsies (with benign and malignant outcome) based on age</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-visualize-the-biopsies-(with-benign-and-malignant-outcome)-based-on-age">Let's visualize the biopsies (with benign and malignant outcome) based on age<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-visualize-the-biopsies-(with-benign-and-malignant-outcome)-based-on-age">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[11]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 251.681px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true" style="right: 0px; left: 46px; display: block;"><div style="height: 100%; min-height: 1px; width: 632px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 631.958px; margin-bottom: -19px; border-right-width: 11px; min-height: 113px; padding-right: 0px; padding-bottom: 19px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>6</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 205.681px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation" style=""><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">plt</span>.<span class="cm-property">figure</span><span class=" CodeMirror-matchingbracket">(</span><span class="cm-variable">figsize</span><span class="cm-operator">=</span>(<span class="cm-number">10</span>,<span class="cm-number">6</span>)<span class=" CodeMirror-matchingbracket">)</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">df</span>[<span class="cm-variable">df</span>[<span class="cm-string">'Target'</span>]<span class="cm-operator">==</span><span class="cm-number">1</span>][<span class="cm-string">'Age'</span>].<span class="cm-property">hist</span>(<span class="cm-variable">alpha</span><span class="cm-operator">=</span><span class="cm-number">0.5</span>,<span class="cm-variable">color</span><span class="cm-operator">=</span><span class="cm-string">'blue'</span>,<span class="cm-variable">bins</span><span class="cm-operator">=</span><span class="cm-number">30</span>,<span class="cm-variable">label</span><span class="cm-operator">=</span><span class="cm-string">'Malignant'</span>)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">3</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">df</span>[<span class="cm-variable">df</span>[<span class="cm-string">'Target'</span>]<span class="cm-operator">==</span><span class="cm-number">0</span>][<span class="cm-string">'Age'</span>].<span class="cm-property">hist</span>(<span class="cm-variable">alpha</span><span class="cm-operator">=</span><span class="cm-number">0.5</span>,<span class="cm-variable">color</span><span class="cm-operator">=</span><span class="cm-string">'red'</span>,<span class="cm-variable">bins</span><span class="cm-operator">=</span><span class="cm-number">30</span>,<span class="cm-variable">label</span><span class="cm-operator">=</span><span class="cm-string">'Benign'</span>)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">4</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">plt</span>.<span class="cm-property">legend</span>()</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">5</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">plt</span>.<span class="cm-property">xlabel</span>(<span class="cm-string">'Age'</span>)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">6</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">plt</span>.<span class="cm-property">show</span>()</span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 19px solid transparent; top: 113px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 143px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt"></div><div class="output_subarea output_png"><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlMAAAF3CAYAAACBlM5VAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDMuMC4xLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvDW2N/gAAIABJREFUeJzt3X+Q3XV97/Hn2xASZPcSSCATiG1gajeUXwlZIj+scxZbRGUQvTAYvRYvSri3UFNuuVacERfUGTvtKLe39c6NhcKdVhYLOlKK/ChyQFolEAmQkMQUmrbRyE/BrCWR0Pf9Y09iSHazm/M5Z885Oc/HzE7O+Z7v93zf+97z45XP93s+JzITSZIk1edNrS5AkiSpkxmmJEmSChimJEmSChimJEmSChimJEmSChimJEmSChimJEmSChimJEmSChimJEmSChimJEmSChwwmTubNWtWzps3bzJ3OSE///nPOfjgg1tdRkvZA3sA9gDsAdiDHeyDPVi5cuULmXn4eOtNapiaN28ejz766GTuckKq1SqVSqXVZbSUPbAHYA/AHoA92ME+2IOI+JeJrOdhPkmSpAKGKUmSpAKGKUmSpAKTes6UJEkq89prr7Fp0ya2bt3a9H0dcsghrF27tun7abXp06czd+5cpk6dWtf2hilJkjrIpk2b6O3tZd68eUREU/e1ZcsWent7m7qPVstMXnzxRTZt2sTRRx9d1314mE+SpA6ydetWZs6c2fQg1S0igpkzZxaN9BmmJEnqMAapxirtp2FKkiTtk4jgIx/5yM7r27dv5/DDD+ecc87Z63bVanXnOrfffjtf/OIXm1rnrlatWsWdd97ZlPv2nClJkjrY4ODk39/BBx/M6tWrefXVVznooIO49957Oeqoo/ZpP+eeey7nnntufUXWYdWqVTz66KO85z3vafh9OzIlSZL22bvf/W7+7u/+DoCbb76ZJUuW7LxtxYoVnH766SxcuJDTTz+d9evX77H9jTfeyOWXXw7A008/zamnnsopp5zC1VdfTU9PD/DLGdjPP/985s+fz4c//GEyE4Brr72WU045heOPP56lS5fuXF6pVPjDP/xDFi9ezK//+q/z3e9+l1/84hdcffXV3HLLLSxYsIBbbrmlob0wTEmSpH32wQ9+kKGhIbZu3coTTzzB2972tp23zZ8/nwcffJDHHnuMa6+9lk9/+tN7va9ly5axbNkyHnnkEY488sg33PbYY49x3XXX8dRTT/HMM8/wD//wDwBcfvnlPPLIIztHyO64446d22zfvp0VK1Zw3XXXcc0113DggQdy7bXXcuGFF7Jq1SouvPDCBnbCMCVJkupw4oknsnHjRm6++eY9Dp298sorXHDBBRx//PFcccUVrFmzZq/39b3vfY8LLrgAgA996ENvuG3x4sXMnTuXN73pTSxYsICNGzcCcP/99/O2t72NE044ge985ztv2McHPvABABYtWrRz/WYyTEmSpLqce+65XHnllW84xAfwmc98hoGBAVavXs3f/u3fFk07MG3atJ2Xp0yZwvbt29m6dSu/+7u/y6233sqTTz7JJZdc8oZ97Nhmx/rN5gnoktShSk48bvRJy+pOF198MYcccggnnHAC1Wp15/JXXnll5wnpN95447j3c+qpp3Lbbbdx4YUXMjQ0NO76O4LTrFmzGB4e5tZbb+X888/f6za9vb1s2bJl3PuuhyNTkiSpLnPnzmXZsmV7LP/kJz/JVVddxRlnnMHrr78+7v1cd911fOlLX2Lx4sVs3ryZQw45ZK/rz5gxg0suuYQTTjiB8847j1NOOWXcfQwMDPDUU0815QR0R6YkSepgrRhlHB4e3mNZpVKhUqkAcNppp/HDH/5w522f+9zn9ljnox/9KB/96EcBOOqoo/j+979PRDA0NER/f/8e6wP82Z/92c7Ln//85/n85z+/Rx27jpDNmjVr5zlThx12GI888si+/qoTYpiSJEkttXLlSi6//HIykxkzZnDDDTe0uqR9YpiSJEkt9Zu/+Zs8/vjjrS6jbp4zJUmSVMAwJUmSVMAwJUmSVMAwJUmSVMAwJUmS9smUKVNYsGABJ510EieffDL/+I//WPd9XX311fz93/99A6ubfH6aT5KkTtboiaYmcH8HHXQQq1atAuDuu+/mqquu4oEHHqhrd9dee21d27UTR6YkSVLdfvazn3HooYfuvP7Hf/zHnHLKKZx44ol89rOfBWDjxo0ce+yxXHLJJRx33HGcddZZvPrqq8DI5J233norAHfeeSfz58/n7W9/O5/4xCc455xzABgcHOTiiy+mUqlwzDHH8Kd/+qeT/FvunWFKkiTtk1dffZUFCxYwf/58Pv7xj/OZz3wGgHvuuYcNGzawYsUK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"></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 23.375px; left: 75.3889px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 47px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 41.3889px; top: 17.7778px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's do the "train test split" using test size = 0.3 and random states default value</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 47px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 58px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-do-the-&quot;train-test-split&quot;-using-test-size-=-0.3-and-random-states-default-value">Let's do the "train test split" using test size = 0.3 and random states default value<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-do-the-%22train-test-split%22-using-test-size-=-0.3-and-random-states-default-value">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[12]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 267.097px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 416.333px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><pre>x</pre></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"><div class="CodeMirror-selected" style="position: absolute; left: 105.778px; top: 0px; width: 115.319px; height: 17px;"></div></div><div class="CodeMirror-cursors" style="visibility: hidden;"></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-keyword">from</span> <span class="cm-variable">sklearn</span>.<span class="cm-property">model_selection</span> <span class="cm-keyword">import</span>  <span class="cm-variable">train_test_split</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[13]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 22.4861px; left: 174.722px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 231.75px; margin-bottom: -19px; border-right-width: 11px; min-height: 45px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><pre><span>xxxxxxxxxx</span></pre></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 128.722px; top: 16.8889px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">X</span> <span class="cm-operator">=</span> <span class="cm-variable">df</span>.<span class="cm-property">drop</span>(<span class="cm-string">'Target'</span>, <span class="cm-variable">axis</span><span class="cm-operator">=</span><span class="cm-number">1</span>)</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">y</span> <span class="cm-operator">=</span> <span class="cm-variable">df</span><span class=" CodeMirror-matchingbracket">[</span><span class="cm-string">'Target'</span><span class=" CodeMirror-matchingbracket">]</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 45px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 56px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[27]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 22.4861px; left: 624px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true" style="right: 0px; left: 47px; display: block;"><div style="height: 100%; min-height: 1px; width: 685px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 685.889px; margin-bottom: -19px; border-right-width: 11px; min-height: 45px; padding-right: 0px; padding-bottom: 19px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>2</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 675.194px; top: 16.8889px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div><div class="CodeMirror-gutter-elt" style="left: 33px; width: 12px;"><div class="CodeMirror-foldgutter-open CodeMirror-guttermarker-subtle"></div></div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">X_train</span>, <span class="cm-variable">X_test</span>, <span class="cm-variable">y_train</span>, <span class="cm-variable">y_test</span> <span class="cm-operator">=</span> <span class="cm-variable">train_test_split</span><span class=" CodeMirror-matchingbracket">(</span><span class="cm-variable">X</span>,</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;">                                                    <span class="cm-variable">y</span>, <span class="cm-variable">test_size</span><span class="cm-operator">=</span><span class="cm-number">0.30</span>, <span class="cm-variable">random_state</span><span class="cm-operator">=</span><span class="cm-number">101</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 19px solid transparent; top: 45px;"></div><div class="CodeMirror-gutters" style="left: 1px; height: 75px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h3"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.5972px; left: 191.333px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true" style="bottom: 0px;"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 30px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 157.333px; top: 0px; height: 18.6667px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-3">### Decision Tree</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 30px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 41px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h3 id="Decision-Tree" data-toc-modified-id="Decision-Tree-1.0.2"><a class="toc-mod-link" id="Decision-Tree-1.0.2"></a><span class="toc-item-num">1.0.2&nbsp;&nbsp;</span>Decision Tree<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Decision-Tree">¶</a></h3>
</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 471.333px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 437.333px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's import decision tree classifier and create its instance</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-import-decision-tree-classifier-and-create-its-instance">Let's import decision tree classifier and create its instance<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-import-decision-tree-classifier-and-create-its-instance">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[28]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 22.4861px; left: 297.889px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 370.111px; margin-bottom: -19px; border-right-width: 11px; min-height: 45px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>2</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 251.889px; top: 16.8889px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-keyword">from</span> <span class="cm-variable">sklearn</span>.<span class="cm-property">tree</span> <span class="cm-keyword">import</span> <span class="cm-variable">DecisionTreeClassifier</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">dtree</span> <span class="cm-operator">=</span> <span class="cm-variable">DecisionTreeClassifier</span><span class=" CodeMirror-matchingbracket">(</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 45px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 56px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 331.528px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 297.528px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's fit the training data to the model</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-fit-the-training-data-to-the-model">Let's fit the training data to the model<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-fit-the-training-data-to-the-model">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[29]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 259.389px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 216.389px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 213.389px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">dtree</span>.<span class="cm-property">fit</span><span class=" CodeMirror-matchingbracket">(</span><span class="cm-variable">X_train</span>, <span class="cm-variable">y_train</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt output_prompt"><bdi>Out[29]:</bdi></div><div class="output_subarea output_text output_result"><pre>DecisionTreeClassifier(class_weight=None, criterion='gini', max_depth=None,
            max_features=None, max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=1, min_samples_split=2,
            min_weight_fraction_leaf=0.0, presort=False, random_state=None,
            splitter='best')</pre></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 354.097px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><pre><span>xxxxxxxxxx</span></pre></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 320.097px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's do the predictions for our test data </span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-do-the-predictions-for-our-test-data">Let's do the predictions for our test data<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-do-the-predictions-for-our-test-data">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[30]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 320.972px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 277.972px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 274.972px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">predictions</span> <span class="cm-operator">=</span> <span class="cm-variable">dtree</span>.<span class="cm-property">predict</span><span class=" CodeMirror-matchingbracket">(</span><span class="cm-variable">X_test</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h6"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 39.3333px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors"><div class="CodeMirror-cursor" style="left: 5.33333px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-6">###### Let's import ther Classification report and Confusion matrix then print them</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h6 id="Let&#39;s-import-ther-Classification-report-and-Confusion-matrix-then-print-them">Let's import ther Classification report and Confusion matrix then print them<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-import-ther-Classification-report-and-Confusion-matrix-then-print-them">¶</a></h6>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[31]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 566.889px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 523.889px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 520.889px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-keyword">from</span> <span class="cm-variable">sklearn</span>.<span class="cm-property">metrics</span> <span class="cm-keyword">import</span> <span class="cm-variable">confusion_matrix</span>, <span class="cm-variable">classification_report</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[32]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 22.4861px; left: 428.736px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 385.736px; margin-bottom: -19px; border-right-width: 11px; min-height: 45px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>2</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 382.736px; top: 16.8889px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-builtin">print</span>(<span class="cm-variable">confusion_matrix</span>(<span class="cm-variable">y_test</span>, <span class="cm-variable">predictions</span>))</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-builtin">print</span><span class=" CodeMirror-matchingbracket">(</span><span class="cm-variable">classification_report</span>(<span class="cm-variable">y_test</span>, <span class="cm-variable">predictions</span>)<span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 45px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 56px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt"></div><div class="output_subarea output_text output_stream output_stdout"><pre>[[106  27]
 [ 42  74]]
              precision    recall  f1-score   support

           0       0.72      0.80      0.75       133
           1       0.73      0.64      0.68       116

   micro avg       0.72      0.72      0.72       249
   macro avg       0.72      0.72      0.72       249
weighted avg       0.72      0.72      0.72       249

</pre></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h3"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 216.222px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true" style="bottom: 0px;"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 30px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 182.222px; top: 0px; height: 18.6667px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-3">### Random Forests</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 30px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 41px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h3 id="Random-Forests" data-toc-modified-id="Random-Forests-1.0.3"><a class="toc-mod-link" id="Random-Forests-1.0.3"></a><span class="toc-item-num">1.0.3&nbsp;&nbsp;</span>Random Forests<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Random-Forests">¶</a></h3>
</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 39.3333px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 47px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors"><div class="CodeMirror-cursor" style="left: 5.33333px; top: 0px; height: 17.7777px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's import Random Forest Classifier and create its instance with 200 no of trees</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 47px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 58px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-import-Random-Forest-Classifier-and-create-its-instance-with-200-no-of-trees">Let's import Random Forest Classifier and create its instance with 200 no of trees<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-import-Random-Forest-Classifier-and-create-its-instance-with-200-no-of-trees">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[33]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 22.4861px; left: 405.625px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1" draggable="false"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 401.111px; margin-bottom: -19px; border-right-width: 11px; min-height: 45px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>2</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 359.625px; top: 16.8889px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-keyword">from</span> <span class="cm-variable">sklearn</span>.<span class="cm-property">ensemble</span> <span class="cm-keyword">import</span> <span class="cm-variable">RandomForestClassifier</span></span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">rfc</span> <span class="cm-operator">=</span> <span class="cm-variable">RandomForestClassifier</span><span class=" CodeMirror-matchingbracket">(</span><span class="cm-variable">n_estimators</span><span class="cm-operator">=</span><span class="cm-number">200</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 45px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 56px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 438.111px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 404.111px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's fit our training data to Random Forests instance</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-fit-our-training-data-to-Random-Forests-instance">Let's fit our training data to Random Forests instance<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-fit-our-training-data-to-Random-Forests-instance">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[34]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 236.292px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 200.986px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 190.292px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">rfc</span>.<span class="cm-property">fit</span><span class=" CodeMirror-matchingbracket">(</span><span class="cm-variable">X_train</span>, <span class="cm-variable">y_train</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt output_prompt"><bdi>Out[34]:</bdi></div><div class="output_subarea output_text output_result"><pre>RandomForestClassifier(bootstrap=True, class_weight=None, criterion='gini',
            max_depth=None, max_features='auto', max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=1, min_samples_split=2,
            min_weight_fraction_leaf=0.0, n_estimators=200, n_jobs=None,
            oob_score=False, random_state=None, verbose=0,
            warm_start=False)</pre></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 239.333px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 205.333px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's do the predictions</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-do-the-predictions">Let's do the predictions<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-do-the-predictions">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[35]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 336.361px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 293.361px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 290.361px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-variable">rfc_predictions</span> <span class="cm-operator">=</span> <span class="cm-variable">rfc</span>.<span class="cm-property">predict</span><span class=" CodeMirror-matchingbracket">(</span><span class="cm-variable">X_test</span><span class=" CodeMirror-matchingbracket">)</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered unselected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h5"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59723px; left: 224.208px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 190.208px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-5">##### Let's print the results</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h5 id="Let&#39;s-print-the-results">Let's print the results<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#Let&#39;s-print-the-results">¶</a></h5>
</div></div></div><div class="cell code_cell rendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[36]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 22.4861px; left: 444.139px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 416.528px; margin-bottom: -19px; border-right-width: 11px; min-height: 45px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>2</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style="visibility: hidden;"><div class="CodeMirror-cursor" style="left: 398.139px; top: 16.8889px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-builtin">print</span>(<span class="cm-variable">confusion_matrix</span>(<span class="cm-variable">y_test</span>, <span class="cm-variable">rfc_predictions</span>))</span></pre></div><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">2</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-builtin">print</span>(<span class="cm-variable">classification_report</span><span class=" CodeMirror-matchingbracket">(</span><span class="cm-variable">y_test</span>, <span class="cm-variable">rfc_predictions</span><span class=" CodeMirror-matchingbracket">)</span>)</span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 45px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 56px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to scroll output; double click to hide"></div><div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt"></div><div class="output_subarea output_text output_stream output_stdout"><pre>[[110  23]
 [ 30  86]]
              precision    recall  f1-score   support

           0       0.79      0.83      0.81       133
           1       0.79      0.74      0.76       116

   micro avg       0.79      0.79      0.79       249
   macro avg       0.79      0.78      0.79       249
weighted avg       0.79      0.79      0.79       249

</pre></div></div></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div><div class="cell text_cell collapsible_headings_ellipsis rendered selected" tabindex="2"><div class="prompt input_prompt"><div class="collapsible_headings_toggle" style="color: rgb(170, 170, 170);"><div class="h6"><i class="fa fa-fw fa-caret-down"></i></div></div></div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-default CodeMirror-wrap"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59717px; left: 427.778px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 34px; margin-bottom: -19px; border-right-width: 11px; min-height: 29px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 393.778px; top: 0px; height: 17.7778px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -34px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span class="cm-header cm-header-6">###### The Random Forests Classifier are the best model.</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 29px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 40px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div></div></div></div></div><div class="text_cell_render rendered_html" tabindex="-1"><h6 id="The-Random-Forests-Classifier-are-the-best-model.">The Random Forests Classifier are the best model.<a class="anchor-link" href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#The-Random-Forests-Classifier-are-the-best-model.">¶</a></h6>
</div></div></div><div class="cell code_cell unrendered unselected" tabindex="2"><div class="input"><div class="run_this_cell" title="Run this cell"><i class="fa-step-forward fa"></i></div><div class="prompt input_prompt"><bdi>In</bdi>&nbsp;[&nbsp;]:</div><div class="inner_cell"><div class="ctb_hideshow"><div class="celltoolbar"></div></div><div class="input_area"><div class="CodeMirror cm-s-ipython"><div style="overflow: hidden; position: relative; width: 3px; height: 0px; top: 5.59766px; left: 51.5972px;"><textarea autocorrect="off" autocapitalize="off" spellcheck="false" tabindex="0" style="position: absolute; bottom: -1em; padding: 0px; width: 1000px; height: 1em; outline: none;"></textarea></div><div class="CodeMirror-vscrollbar" cm-not-content="true"><div style="min-width: 1px; height: 0px;"></div></div><div class="CodeMirror-hscrollbar" cm-not-content="true"><div style="height: 100%; min-height: 1px; width: 0px;"></div></div><div class="CodeMirror-scrollbar-filler" cm-not-content="true"></div><div class="CodeMirror-gutter-filler" cm-not-content="true"></div><div class="CodeMirror-scroll" tabindex="-1"><div class="CodeMirror-sizer" style="margin-left: 46px; min-width: 8.59723px; margin-bottom: -19px; border-right-width: 11px; min-height: 28px; padding-right: 0px; padding-bottom: 0px;"><div style="position: relative; top: 0px;"><div class="CodeMirror-lines" role="presentation"><div role="presentation" style="position: relative; outline: none;"><div class="CodeMirror-measure"><div class="CodeMirror-linenumber CodeMirror-gutter-elt"><div>1</div></div></div><div class="CodeMirror-measure"></div><div style="position: relative; z-index: 1;"></div><div class="CodeMirror-cursors" style=""><div class="CodeMirror-cursor" style="left: 5.59723px; top: 0px; height: 16.8889px;">&nbsp;</div></div><div class="CodeMirror-code" role="presentation"><div style="position: relative;"><div class="CodeMirror-gutter-wrapper" style="left: -46px;"><div class="CodeMirror-linenumber CodeMirror-gutter-elt" style="left: 0px; width: 21px;">1</div></div><pre class=" CodeMirror-line " role="presentation"><span role="presentation" style="padding-right: 0.1px;"><span cm-text="">​</span></span></pre></div></div></div></div></div></div><div style="position: absolute; height: 11px; width: 1px; border-bottom: 0px solid transparent; top: 28px;"></div><div class="CodeMirror-gutters" style="left: 0px; height: 39px;"><div class="CodeMirror-gutter CodeMirror-linenumbers" style="width: 33px;"></div><div class="CodeMirror-gutter CodeMirror-foldgutter"></div></div></div></div></div></div></div><div class="output_wrapper"><div class="out_prompt_overlay prompt" title="click to expand output; double click to hide output"></div><div class="output"></div><div class="btn btn-default output_collapsed" title="click to expand output" style="display: none;">. . .</div></div></div></div><div class="end_space"></div></div>
        <div id="tooltip" class="ipython_tooltip" style="left: 143.875px; top: 3020.68px; display: none;"><div class="tooltipbuttons"><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" role="button" class="ui-button"><span class="ui-icon ui-icon-close">Close</span></a><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="ui-corner-all" role="button" id="expanbutton" title="Grow the tooltip vertically (press shift-tab twice)" style=""><span class="ui-icon ui-icon-plus">Expand</span></a><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" role="button" class="ui-button" title="show the current docstring in pager (press shift-tab 4 times)"><span class="ui-icon ui-icon-arrowstop-l-n">Open in Pager</span></a><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" role="button" class="ui-button" title="Tooltip will linger for 10 seconds while you type" style="display: none;"><span class="ui-icon ui-icon-clock">Close</span></a></div><div class="pretooltiparrow" style="left: 104.75px;"></div><div class="tooltiptext smalltooltip"><pre><span class="ansi-red-intense-fg ansi-bold">Signature:</span> train_test_split<span class="ansi-yellow-intense-fg ansi-bold">(</span><span class="ansi-yellow-intense-fg ansi-bold">*</span>arrays<span class="ansi-yellow-intense-fg ansi-bold">,</span> <span class="ansi-yellow-intense-fg ansi-bold">**</span>options<span class="ansi-yellow-intense-fg ansi-bold">)</span>
<span class="ansi-red-intense-fg ansi-bold">Docstring:</span>
Split arrays or matrices into random train and test subsets

Quick utility that wraps input validation and
``next(ShuffleSplit().split(X, y))`` and application to input data
into a single call for splitting (and optionally subsampling) data in a
oneliner.

Read more in the :ref:`User Guide &lt;cross_validation&gt;`.

Parameters
----------
*arrays : sequence of indexables with same length / shape[0]
    Allowed inputs are lists, numpy arrays, scipy-sparse
    matrices or pandas dataframes.

test_size : float, int or None, optional (default=0.25)
    If float, should be between 0.0 and 1.0 and represent the proportion
    of the dataset to include in the test split. If int, represents the
    absolute number of test samples. If None, the value is set to the
    complement of the train size. By default, the value is set to 0.25.
    The default will change in version 0.21. It will remain 0.25 only
    if ``train_size`` is unspecified, otherwise it will complement
    the specified ``train_size``.

train_size : float, int, or None, (default=None)
    If float, should be between 0.0 and 1.0 and represent the
    proportion of the dataset to include in the train split. If
    int, represents the absolute number of train samples. If None,
    the value is automatically set to the complement of the test size.

random_state : int, RandomState instance or None, optional (default=None)
    If int, random_state is the seed used by the random number generator;
    If RandomState instance, random_state is the random number generator;
    If None, the random number generator is the RandomState instance used
    by `np.random`.

shuffle : boolean, optional (default=True)
    Whether or not to shuffle the data before splitting. If shuffle=False
    then stratify must be None.

stratify : array-like or None (default=None)
    If not None, data is split in a stratified fashion, using this as
    the class labels.

Returns
-------
splitting : list, length=2 * len(arrays)
    List containing train-test split of inputs.

    .. versionadded:: 0.16
        If the input is sparse, the output will be a
        ``scipy.sparse.csr_matrix``. Else, output type is the same as the
        input type.

Examples
--------
&gt;&gt;&gt; import numpy as np
&gt;&gt;&gt; from sklearn.model_selection import train_test_split
&gt;&gt;&gt; X, y = np.arange(10).reshape((5, 2)), range(5)
&gt;&gt;&gt; X
array([[0, 1],
       [2, 3],
       [4, 5],
       [6, 7],
       [8, 9]])
&gt;&gt;&gt; list(y)
[0, 1, 2, 3, 4]

&gt;&gt;&gt; X_train, X_test, y_train, y_test = train_test_split(
...     X, y, test_size=0.33, random_state=42)
...
&gt;&gt;&gt; X_train
array([[4, 5],
       [0, 1],
       [6, 7]])
&gt;&gt;&gt; y_train
[2, 0, 3]
&gt;&gt;&gt; X_test
array([[2, 3],
       [8, 9]])
&gt;&gt;&gt; y_test
[1, 4]

&gt;&gt;&gt; train_test_split(y, shuffle=False)
[[0, 1, 2], [3, 4]]
<span class="ansi-red-intense-fg ansi-bold">File:</span>      c:\users\stevy\anaconda3\lib\site-packages\sklearn\model_selection\_split.py
<span class="ansi-red-intense-fg ansi-bold">Type:</span>      function
</pre></div></div>
    </div>
</div>



</div>



<div id="pager" class="ui-resizable">
    <div id="pager-contents">
        <div id="pager-container" class="container"></div>
    </div>
    <div id="pager-button-area"><a role="button" title="Open the pager in an external window" class="ui-button"><span class="ui-icon ui-icon-extlink"></span></a><a role="button" title="Close the pager" class="ui-button"><span class="ui-icon ui-icon-close"></span></a></div>
<div class="ui-resizable-handle ui-resizable-n" style="z-index: 90;"></div></div>






<script type="text/javascript">
    sys_info = {"notebook_version": "5.6.0", "notebook_path": "C:\\Users\\Stevy\\Anaconda3\\lib\\site-packages\\notebook", "commit_source": "", "commit_hash": "", "sys_version": "3.7.0 (default, Jun 28 2018, 08:04:48) [MSC v.1912 64 bit (AMD64)]", "sys_executable": "C:\\Users\\Stevy\\Anaconda3\\python.exe", "sys_platform": "win32", "platform": "Windows-10-10.0.17134-SP0", "os_name": "nt", "default_encoding": "utf-8"};
</script>

<script src="./README_files/encoding.js.download" charset="utf-8"></script>

<script src="./README_files/main.min.js.download" type="text/javascript" charset="utf-8"></script>



<script type="text/javascript">
  function _remove_token_from_url() {
    if (window.location.search.length <= 1) {
      return;
    }
    var search_parameters = window.location.search.slice(1).split('&');
    for (var i = 0; i < search_parameters.length; i++) {
      if (search_parameters[i].split('=')[0] === 'token') {
        // remote token from search parameters
        search_parameters.splice(i, 1);
        var new_search = '';
        if (search_parameters.length) {
          new_search = '?' + search_parameters.join('&');
        }
        var new_url = window.location.origin + 
                      window.location.pathname + 
                      new_search + 
                      window.location.hash;
        window.history.replaceState({}, "", new_url);
        return;
      }
    }
  }
  _remove_token_from_url();
</script>


<div style="position: absolute; width: 0px; height: 0px; overflow: hidden; padding: 0px; border: 0px; margin: 0px;"><div id="MathJax_Font_Test" style="position: absolute; visibility: hidden; top: 0px; left: 0px; width: auto; min-width: 0px; max-width: none; padding: 0px; border: 0px; margin: 0px; white-space: nowrap; text-align: left; text-indent: 0px; text-transform: none; line-height: normal; letter-spacing: normal; word-spacing: normal; font-size: 40px; font-weight: normal; font-style: normal; font-family: STIXMathJax_Main, sans-serif;"></div></div><div id="varInspector-wrapper" style="position: fixed; display: none; max-height: 727px;" class="ui-draggable ui-resizable varInspector-float-wrapper"><div id="varInspector-header" class="header">Variable Inspector <a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="kill-btn" title="Close window">[x]</a><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="hide-btn" title="Hide Variable Inspector">[-]</a><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" class="reload-btn" title="Reload Variable Inspector">  ↻</a><span>&nbsp;&nbsp;</span><span>&nbsp;&nbsp;</span></div><div id="varInspector" class="varInspector" style="height: 92.222px; max-height: 707px;"><div class="inspector"><table class="table fixed table-condensed table-nonfluid tablesorter tablesorter-default" role="grid"><colgroup><col>  <col><col></colgroup><thead><tr role="row" class="tablesorter-headerRow"><th data-column="0" class="tablesorter-header tablesorter-headerUnSorted" tabindex="0" scope="col" role="columnheader" aria-disabled="false" unselectable="on" aria-sort="none" aria-label="X: No sort applied, activate to apply an ascending sort" style="user-select: none;"><div class="tablesorter-header-inner">X</div></th><th data-column="1" class="tablesorter-header tablesorter-headerUnSorted" tabindex="0" scope="col" role="columnheader" aria-disabled="false" unselectable="on" aria-sort="none" aria-label="Name: No sort applied, activate to apply an ascending sort" style="user-select: none;"><div class="tablesorter-header-inner">Name</div></th><th data-column="2" class="tablesorter-header tablesorter-headerUnSorted" tabindex="0" scope="col" role="columnheader" aria-disabled="false" unselectable="on" aria-sort="none" aria-label="Type: No sort applied, activate to apply an ascending sort" style="user-select: none;"><div class="tablesorter-header-inner">Type</div></th><th data-column="3" class="tablesorter-header tablesorter-headerUnSorted" tabindex="0" scope="col" role="columnheader" aria-disabled="false" unselectable="on" aria-sort="none" aria-label="Size: No sort applied, activate to apply an ascending sort" style="user-select: none;"><div class="tablesorter-header-inner">Size</div></th><th data-column="4" class="tablesorter-header tablesorter-headerUnSorted" tabindex="0" scope="col" role="columnheader" aria-disabled="false" unselectable="on" aria-sort="none" aria-label="Shape: No sort applied, activate to apply an ascending sort" style="user-select: none;"><div class="tablesorter-header-inner">Shape</div></th><th data-column="5" class="tablesorter-header tablesorter-headerUnSorted" tabindex="0" scope="col" role="columnheader" aria-disabled="false" unselectable="on" aria-sort="none" aria-label="Value: No sort applied, activate to apply an ascending sort" style="user-select: none;"><div class="tablesorter-header-inner">Value</div></th></tr></thead><tbody aria-live="polite" aria-relevant="all"><tr role="row"><td>  </td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del DecisionTreeClassifier&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>DecisionTreeC...</td><td>ABCMeta</td><td>2000</td><td></td><td><class 'sklearn.tree.tree.decisiontre...<="" td=""></class></td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del RandomForestClassifier&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>RandomForestC...</td><td>ABCMeta</td><td>888</td><td></td><td><class 'sklearn.ensemble.forest.rando...<="" td=""></class></td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del X&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>X</td><td>DataFrame</td><td>33280</td><td>(830, 5)</td><td>     BI-RADS  Age  Shape  Margin  Den...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del X_test&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>X_test</td><td>DataFrame</td><td>11952</td><td>(249, 5)</td><td>     BI-RADS  Age  Shape  Margin  Den...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del X_train&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>X_train</td><td>DataFrame</td><td>27888</td><td>(581, 5)</td><td>     BI-RADS  Age  Shape  Margin  Den...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del df&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>df</td><td>DataFrame</td><td>39920</td><td>(830, 6)</td><td>     BI-RADS  Age  Shape  Margin  Den...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del dtree&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>dtree</td><td>DecisionTreeC...</td><td>56</td><td></td><td>DecisionTreeClassifier(class_weight=N...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del predictions&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>predictions</td><td>ndarray</td><td>1992</td><td>(249,)</td><td>[1 0 1 0 0 0 1 0 1 0 1 0 0 0 0 0 1 0 ...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del rdc&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>rdc</td><td>RandomForestC...</td><td>56</td><td></td><td>RandomForestClassifier(bootstrap=True...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del rfc&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>rfc</td><td>RandomForestC...</td><td>56</td><td></td><td>RandomForestClassifier(bootstrap=True...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del rfc_predictions&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>rfc_predictions</td><td>ndarray</td><td>1992</td><td>(249,)</td><td>[1 0 1 0 1 0 1 0 1 0 1 0 1 0 0 0 1 0 ...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del y&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>y</td><td>Series</td><td>6640</td><td>(830,)</td><td>0      1
1      1
2      0
3      1
4...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del y_test&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>y_test</td><td>Series</td><td>1992</td><td>(249,)</td><td>182    1
268    0
730    1
624    0
5...</td></tr><tr role="row"><td><a href="http://localhost:8888/notebooks/Mammographic_Masses_Dataset.ipynb#" onclick="Jupyter.notebook.kernel.execute(&#39;del y_train&#39;); Jupyter.notebook.events.trigger(&#39;varRefresh&#39;); ">x</a></td><td>y_train</td><td>Series</td><td>4648</td><td>(581,)</td><td>148    0
573    0
4      1
274    1
5...</td></tr></tbody></table></div></div><div class="ui-resizable-handle ui-resizable-e" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-s" style="z-index: 90;"></div><div class="ui-resizable-handle ui-resizable-se ui-icon ui-icon-gripsmall-diagonal-se" style="z-index: 90;"></div></div><style>#toc li > span:hover { background-color: #DAA520 }
.toc-item-highlight-select  {background-color: #FFD700}
.toc-item-highlight-execute  {background-color: #FF0000}
.toc-item-highlight-execute.toc-item-highlight-select   {background-color: #FFD700}div#menubar-container, div#header-container {
width: auto;
padding-left: 20px; }#toc-wrapper { background-color: #FFFFFF}
#toc a, #navigate_menu a, .toc { color: #333333}#toc-wrapper .toc-item-num { color: #000000}.sidebar-wrapper { border-color: #EEEEEE}.highlight_on_scroll { border-left: solid 4px #2447f0}</style><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 1</strong>  <div title="jupyter-notebook:change-cell-to-heading-1" class="pull-right command-shortcut"><kbd>1</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 2</strong>  <div title="jupyter-notebook:change-cell-to-heading-2" class="pull-right command-shortcut"><kbd>2</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query"><input type="search"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i>automatically indent selection  <div title="jupyter-notebook:auto-indent" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="1"><i class="fa fa-icon {{icon}}"></i>change cell to code  <div title="jupyter-notebook:change-cell-to-code" class="pull-right command-shortcut"><kbd>Y</kbd></div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="2"><i class="fa fa-icon {{icon}}"></i>change cell to heading 1  <div title="jupyter-notebook:change-cell-to-heading-1" class="pull-right command-shortcut"><kbd>1</kbd></div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="3"><i class="fa fa-icon {{icon}}"></i>change cell to heading 2  <div title="jupyter-notebook:change-cell-to-heading-2" class="pull-right command-shortcut"><kbd>2</kbd></div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="4"><i class="fa fa-icon {{icon}}"></i>change cell to heading 3  <div title="jupyter-notebook:change-cell-to-heading-3" class="pull-right command-shortcut"><kbd>3</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="5"><i class="fa fa-icon {{icon}}"></i>change cell to heading 4  <div title="jupyter-notebook:change-cell-to-heading-4" class="pull-right command-shortcut"><kbd>4</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="6"><i class="fa fa-icon {{icon}}"></i>change cell to heading 5  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="7"><i class="fa fa-icon {{icon}}"></i>change cell to heading 6  <div title="jupyter-notebook:change-cell-to-heading-6" class="pull-right command-shortcut"><kbd>6</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="8"><i class="fa fa-icon {{icon}}"></i>change cell to markdown  <div title="jupyter-notebook:change-cell-to-markdown" class="pull-right command-shortcut"><kbd>M</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="9"><i class="fa fa-icon {{icon}}"></i>change cell to raw  <div title="jupyter-notebook:change-cell-to-raw" class="pull-right command-shortcut"><kbd>R</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="10"><i class="fa fa-icon {{icon}}"></i>clear all cells output  <div title="jupyter-notebook:clear-all-cells-output" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="11"><i class="fa fa-icon {{icon}}"></i>clear cell output  <div title="jupyter-notebook:clear-cell-output" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="12"><i class="fa fa-icon {{icon}}"></i>close the pager  <div title="jupyter-notebook:close-pager" class="pull-right command-shortcut"><kbd>Esc</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="15"><i class="fa fa-icon fa-repeat"></i>confirm restart kernel  <div title="jupyter-notebook:confirm-restart-kernel" class="pull-right command-shortcut"><kbd>0</kbd>,<kbd>0</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="16"><i class="fa fa-icon {{icon}}"></i>confirm restart kernel and clear output  <div title="jupyter-notebook:confirm-restart-kernel-and-clear-output" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="17"><i class="fa fa-icon fa-forward"></i>confirm restart kernel and run all cells  <div title="jupyter-notebook:confirm-restart-kernel-and-run-all-cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="18"><i class="fa fa-icon fa-repeat"></i>confirm shutdown kernel  <div title="jupyter-notebook:confirm-shutdown-kernel" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="19"><i class="fa fa-icon {{icon}}"></i>copy cell attachments  <div title="jupyter-notebook:copy-cell-attachments" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="20"><i class="fa fa-icon fa-copy"></i>copy selected cells  <div title="jupyter-notebook:copy-cell" class="pull-right command-shortcut"><kbd>C</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="22"><i class="fa fa-icon {{icon}}"></i>cut cell attachments  <div title="jupyter-notebook:cut-cell-attachments" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="23"><i class="fa fa-icon fa-cut"></i>cut selected cells  <div title="jupyter-notebook:cut-cell" class="pull-right command-shortcut"><kbd>X</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="25"><i class="fa fa-icon {{icon}}"></i>delete cells  <div title="jupyter-notebook:delete-cell" class="pull-right command-shortcut"><kbd>D</kbd>,<kbd>D</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="26"><i class="fa fa-icon {{icon}}"></i>duplicate notebook  <div title="jupyter-notebook:duplicate-notebook" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="27"><i class="fa fa-icon {{icon}}"></i>edit command mode keyboard shortcuts  <div title="jupyter-notebook:edit-command-mode-keyboard-shortcuts" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="29"><i class="fa fa-icon {{icon}}"></i>enter command mode  <div title="jupyter-notebook:enter-command-mode" class="pull-right edit-shortcut"><kbd>Esc</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="30"><i class="fa fa-icon {{icon}}"></i>enter edit mode  <div title="jupyter-notebook:enter-edit-mode" class="pull-right command-shortcut"><kbd>Enter</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="31"><i class="fa fa-icon {{icon}}"></i>extend selection above  <div title="jupyter-notebook:extend-selection-above" class="pull-right command-shortcut"><kbd>Shift-K</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="32"><i class="fa fa-icon {{icon}}"></i>extend selection below  <div title="jupyter-notebook:extend-selection-below" class="pull-right command-shortcut"><kbd>Shift-J</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="33"><i class="fa fa-icon {{icon}}"></i>find and replace  <div title="jupyter-notebook:find-and-replace" class="pull-right command-shortcut"><kbd>F</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="34"><i class="fa fa-icon {{icon}}"></i>hide all line numbers  <div title="jupyter-notebook:hide-all-line-numbers" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="35"><i class="fa fa-icon {{icon}}"></i>hide menubar  <div title="jupyter-notebook:hide-menubar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="36"><i class="fa fa-icon {{icon}}"></i>hide the header  <div title="jupyter-notebook:hide-header" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="37"><i class="fa fa-icon {{icon}}"></i>hide the toolbar  <div title="jupyter-notebook:hide-toolbar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="38"><i class="fa fa-icon {{icon}}"></i>ignore  <div title="jupyter-notebook:ignore" class="pull-right command-shortcut"><kbd>Shift</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="40"><i class="fa fa-icon {{icon}}"></i>insert cell above  <div title="jupyter-notebook:insert-cell-above" class="pull-right command-shortcut"><kbd>A</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="41"><i class="fa fa-icon fa-plus"></i>insert cell below  <div title="jupyter-notebook:insert-cell-below" class="pull-right command-shortcut"><kbd>B</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="42"><i class="fa fa-icon {{icon}}"></i>insert image  <div title="jupyter-notebook:insert-image" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="45"><i class="fa fa-icon fa-stop"></i>interrupt the kernel  <div title="jupyter-notebook:interrupt-kernel" class="pull-right command-shortcut"><kbd>I</kbd>,<kbd>I</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="46"><i class="fa fa-icon {{icon}}"></i>merge cell with next cell  <div title="jupyter-notebook:merge-cell-with-next-cell" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="47"><i class="fa fa-icon {{icon}}"></i>merge cell with previous cell  <div title="jupyter-notebook:merge-cell-with-previous-cell" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="48"><i class="fa fa-icon {{icon}}"></i>merge cells  <div title="jupyter-notebook:merge-cells" class="pull-right command-shortcut"><kbd>Shift-M</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="49"><i class="fa fa-icon {{icon}}"></i>merge selected cells  <div title="jupyter-notebook:merge-selected-cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="50"><i class="fa fa-icon fa-arrow-down"></i>move cells down  <div title="jupyter-notebook:move-cell-down" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="51"><i class="fa fa-icon fa-arrow-up"></i>move cells up  <div title="jupyter-notebook:move-cell-up" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="52"><i class="fa fa-icon {{icon}}"></i>move cursor down  <div title="jupyter-notebook:move-cursor-down" class="pull-right edit-shortcut"><kbd>Down</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="53"><i class="fa fa-icon {{icon}}"></i>move cursor up  <div title="jupyter-notebook:move-cursor-up" class="pull-right edit-shortcut"><kbd>Up</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="54"><i class="fa fa-icon {{icon}}"></i>paste cell attachments  <div title="jupyter-notebook:paste-cell-attachments" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="55"><i class="fa fa-icon {{icon}}"></i>paste cell replace  <div title="jupyter-notebook:paste-cell-replace" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="56"><i class="fa fa-icon {{icon}}"></i>paste cells above  <div title="jupyter-notebook:paste-cell-above" class="pull-right command-shortcut"><kbd>Shift-V</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="57"><i class="fa fa-icon fa-paste"></i>paste cells below  <div title="jupyter-notebook:paste-cell-below" class="pull-right command-shortcut"><kbd>V</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="62"><i class="fa fa-icon {{icon}}"></i>rename notebook  <div title="jupyter-notebook:rename-notebook" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="63"><i class="fa fa-icon {{icon}}"></i>restart kernel  <div title="jupyter-notebook:restart-kernel" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="64"><i class="fa fa-icon {{icon}}"></i>restart kernel and clear output  <div title="jupyter-notebook:restart-kernel-and-clear-output" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="65"><i class="fa fa-icon {{icon}}"></i>restart kernel and run all cells  <div title="jupyter-notebook:restart-kernel-and-run-all-cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="66"><i class="fa fa-icon {{icon}}"></i>run all cells  <div title="jupyter-notebook:run-all-cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="67"><i class="fa fa-icon {{icon}}"></i>run all cells above  <div title="jupyter-notebook:run-all-cells-above" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="68"><i class="fa fa-icon {{icon}}"></i>run all cells below  <div title="jupyter-notebook:run-all-cells-below" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="69"><i class="fa fa-icon {{icon}}"></i>run cell and insert below  <div title="jupyter-notebook:run-cell-and-insert-below" class="pull-right command-shortcut"><kbd>Alt-Enter</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="70"><i class="fa fa-icon fa-step-forward"></i>run cell and select next  <div title="jupyter-notebook:run-cell-and-select-next" class="pull-right command-shortcut"><kbd>Shift-Enter</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="71"><i class="fa fa-icon {{icon}}"></i>run selected cells  <div title="jupyter-notebook:run-cell" class="pull-right command-shortcut"><kbd>Ctrl-Enter</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="73"><i class="fa fa-icon fa-save"></i>save notebook  <div title="jupyter-notebook:save-notebook" class="pull-right command-shortcut"><kbd>Ctrl-S</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="76"><i class="fa fa-icon {{icon}}"></i>scroll cell center  <div title="jupyter-notebook:scroll-cell-center" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="77"><i class="fa fa-icon {{icon}}"></i>scroll cell top  <div title="jupyter-notebook:scroll-cell-top" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="78"><i class="fa fa-icon {{icon}}"></i>scroll notebook down  <div title="jupyter-notebook:scroll-notebook-down" class="pull-right command-shortcut"><kbd>Space</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="79"><i class="fa fa-icon {{icon}}"></i>scroll notebook up  <div title="jupyter-notebook:scroll-notebook-up" class="pull-right command-shortcut"><kbd>Shift-Space</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="80"><i class="fa fa-icon {{icon}}"></i>select next cell  <div title="jupyter-notebook:select-next-cell" class="pull-right command-shortcut"><kbd>Down</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="81"><i class="fa fa-icon {{icon}}"></i>select previous cell  <div title="jupyter-notebook:select-previous-cell" class="pull-right command-shortcut"><kbd>Up</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="83"><i class="fa fa-icon {{icon}}"></i>show all line numbers  <div title="jupyter-notebook:show-all-line-numbers" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="84"><i class="fa fa-icon fa-keyboard-o"></i>show command pallette  <div title="jupyter-notebook:show-command-palette" class="pull-right command-shortcut"><kbd>Ctrl-Shift-P</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="85"><i class="fa fa-icon {{icon}}"></i>show keyboard shortcuts  <div title="jupyter-notebook:show-keyboard-shortcuts" class="pull-right command-shortcut"><kbd>H</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="86"><i class="fa fa-icon {{icon}}"></i>show menubar  <div title="jupyter-notebook:show-menubar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="87"><i class="fa fa-icon {{icon}}"></i>show the header  <div title="jupyter-notebook:show-header" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="88"><i class="fa fa-icon {{icon}}"></i>show the toolbar  <div title="jupyter-notebook:show-toolbar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="89"><i class="fa fa-icon {{icon}}"></i>shutdown kernel  <div title="jupyter-notebook:shutdown-kernel" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="90"><i class="fa fa-icon {{icon}}"></i>shutdown kernel and close window  <div title="jupyter-notebook:close-and-halt" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="91"><i class="fa fa-icon {{icon}}"></i>split cell at cursor  <div title="jupyter-notebook:split-cell-at-cursor" class="pull-right edit-shortcut"><kbd>Ctrl-Shift-Minus</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="92"><i class="fa fa-icon {{icon}}"></i>toggle all cells output collapsed  <div title="jupyter-notebook:toggle-all-cells-output-collapsed" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="93"><i class="fa fa-icon {{icon}}"></i>toggle all cells output scrolled  <div title="jupyter-notebook:toggle-all-cells-output-scrolled" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="94"><i class="fa fa-icon fa-list-ol"></i>toggle all line numbers  <div title="jupyter-notebook:toggle-all-line-numbers" class="pull-right command-shortcut"><kbd>Shift-L</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="95"><i class="fa fa-icon {{icon}}"></i>toggle cell output  <div title="jupyter-notebook:toggle-cell-output-collapsed" class="pull-right command-shortcut"><kbd>O</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="96"><i class="fa fa-icon {{icon}}"></i>toggle cell scrolling  <div title="jupyter-notebook:toggle-cell-output-scrolled" class="pull-right command-shortcut"><kbd>Shift-O</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="98"><i class="fa fa-icon {{icon}}"></i>toggle header  <div title="jupyter-notebook:toggle-header" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="99"><i class="fa fa-icon {{icon}}"></i>toggle line numbers  <div title="jupyter-notebook:toggle-cell-line-numbers" class="pull-right command-shortcut"><kbd>L</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="100"><i class="fa fa-icon {{icon}}"></i>toggle menubar  <div title="jupyter-notebook:toggle-menubar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="101"><i class="fa fa-icon {{icon}}"></i>toggle rtl layout  <div title="jupyter-notebook:toggle-rtl-layout" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="103"><i class="fa fa-icon {{icon}}"></i>toggle toolbar  <div title="jupyter-notebook:toggle-toolbar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="107"><i class="fa fa-icon {{icon}}"></i>trust notebook  <div title="jupyter-notebook:trust-notebook" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="110"><i class="fa fa-icon {{icon}}"></i>undo cell deletion  <div title="jupyter-notebook:undo-cell-deletion" class="pull-right command-shortcut"><kbd>Z</kbd></div></a></li><li class="typeahead-group" data-search-group="collapsible_headings"><a href="javascript:;">collapsible_headings command group</a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="13"><i class="fa fa-icon fa-caret-right"></i>collapse-all-headings  <div title="collapsible_headings:collapse_all_headings" class="pull-right command-shortcut"><kbd>Ctrl-Shift-Left</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="14"><i class="fa fa-icon fa-caret-right"></i>collapse-heading  <div title="collapsible_headings:collapse_heading" class="pull-right command-shortcut"><kbd>Left</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="43"><i class="fa fa-icon fa-caret-up"></i>insert-heading-above  <div title="collapsible_headings:insert_heading_above" class="pull-right command-shortcut"><kbd>Shift-A</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="44"><i class="fa fa-icon fa-caret-down"></i>insert-heading-below  <div title="collapsible_headings:insert_heading_below" class="pull-right command-shortcut"><kbd>Shift-B</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="82"><i class="fa fa-icon {{icon}}"></i>select-heading-section  <div title="collapsible_headings:select_heading_section" class="pull-right command-shortcut"><kbd>Shift-Right</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="105"><i class="fa fa-icon fa-angle-double-up"></i>toggle-collapse-all-headings  <div title="collapsible_headings:toggle_collapse_all_headings" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="106"><i class="fa fa-icon fa-angle-double-up"></i>toggle-collapse-heading  <div title="collapsible_headings:toggle_collapse_heading" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="108"><i class="fa fa-icon fa-caret-down"></i>uncollapse-all-headings  <div title="collapsible_headings:uncollapse_all_headings" class="pull-right command-shortcut"><kbd>Ctrl-Shift-Right</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="109"><i class="fa fa-icon fa-caret-down"></i>uncollapse-heading  <div title="collapsible_headings:uncollapse_heading" class="pull-right command-shortcut"><kbd>Right</kbd></div></a></li><li class="typeahead-group" data-search-group="gist_it"><a href="javascript:;">gist_it command group</a></li><li><a href="javascript:;" data-group="gist_it" data-index="21"><i class="fa fa-icon fa-github"></i>create gist from notebook  <div title="gist_it:create-gist-from-notebook" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="code_font_size"><a href="javascript:;">code_font_size command group</a></li><li><a href="javascript:;" data-group="code_font_size" data-index="24"><i class="fa fa-icon fa-search-minus"></i>decrease code font size  <div title="code_font_size:decrease-code-font-size" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="code_font_size" data-index="39"><i class="fa fa-icon fa-search-plus"></i>increase code font size  <div title="code_font_size:increase-code-font-size" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="widgets"><a href="javascript:;">widgets command group</a></li><li><a href="javascript:;" data-group="widgets" data-index="28"><i class="fa fa-icon fa-sliders"></i>embed interactive widgets  <div title="widgets:embed-interactive-widgets" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="widgets" data-index="72"><i class="fa fa-icon fa-truck"></i>save clear widgets  <div title="widgets:save-clear-widgets" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="widgets" data-index="74"><i class="fa fa-icon fa-sliders"></i>save widget state  <div title="widgets:save-widget-state" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="widgets" data-index="75"><i class="fa fa-icon fa-truck"></i>save with widgets  <div title="widgets:save-with-widgets" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="code_prettify"><a href="javascript:;">code_prettify command group</a></li><li><a href="javascript:;" data-group="code_prettify" data-index="58"><i class="fa fa-icon fa-legal"></i>process-all-cells  <div title="code_prettify:process_all_cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="code_prettify" data-index="60"><i class="fa fa-icon fa-legal"></i>process-selected-cells  <div title="code_prettify:process_selected_cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="autopep8"><a href="javascript:;">autopep8 command group</a></li><li><a href="javascript:;" data-group="autopep8" data-index="59"><i class="fa fa-icon fa-legal"></i>process-all-cells  <div title="autopep8:process_all_cells" class="pull-right command-shortcut"><kbd>Ctrl-Shift-L</kbd></div></a></li><li><a href="javascript:;" data-group="autopep8" data-index="61"><i class="fa fa-icon fa-legal"></i>process-selected-cells  <div title="autopep8:process_selected_cells" class="pull-right command-shortcut"><kbd>Ctrl-L</kbd></div></a></li><li class="typeahead-group" data-search-group="auto"><a href="javascript:;">auto command group</a></li><li><a href="javascript:;" data-group="auto" data-index="97"><i class="fa fa-icon fa-comment-o"></i>toggle codefolding  <div title="auto:toggle-codefolding" class="pull-right command-shortcut"><kbd>Alt-F</kbd></div></a></li><li class="typeahead-group" data-search-group="toc2"><a href="javascript:;">toc2 command group</a></li><li><a href="javascript:;" data-group="toc2" data-index="102"><i class="fa fa-icon fa-list"></i>toggle toc  <div title="toc2:toggle-toc" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="varInspector"><a href="javascript:;">varInspector command group</a></li><li><a href="javascript:;" data-group="varInspector" data-index="104"><i class="fa fa-icon fa-crosshairs"></i>toggle variable inspector  <div title="varInspector:toggle-variable-inspector" class="pull-right no-shortcut">{{shortcut}}</div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query"><input type="search"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i>automatically indent selection  <div title="jupyter-notebook:auto-indent" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="1"><i class="fa fa-icon {{icon}}"></i>change cell to code  <div title="jupyter-notebook:change-cell-to-code" class="pull-right command-shortcut"><kbd>Y</kbd></div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="2"><i class="fa fa-icon {{icon}}"></i>change cell to heading 1  <div title="jupyter-notebook:change-cell-to-heading-1" class="pull-right command-shortcut"><kbd>1</kbd></div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="3"><i class="fa fa-icon {{icon}}"></i>change cell to heading 2  <div title="jupyter-notebook:change-cell-to-heading-2" class="pull-right command-shortcut"><kbd>2</kbd></div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="4"><i class="fa fa-icon {{icon}}"></i>change cell to heading 3  <div title="jupyter-notebook:change-cell-to-heading-3" class="pull-right command-shortcut"><kbd>3</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="5"><i class="fa fa-icon {{icon}}"></i>change cell to heading 4  <div title="jupyter-notebook:change-cell-to-heading-4" class="pull-right command-shortcut"><kbd>4</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="6"><i class="fa fa-icon {{icon}}"></i>change cell to heading 5  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="7"><i class="fa fa-icon {{icon}}"></i>change cell to heading 6  <div title="jupyter-notebook:change-cell-to-heading-6" class="pull-right command-shortcut"><kbd>6</kbd></div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="8"><i class="fa fa-icon {{icon}}"></i>change cell to markdown  <div title="jupyter-notebook:change-cell-to-markdown" class="pull-right command-shortcut"><kbd>M</kbd></div></a></li><li class=""><a href="javascript:;" data-group="jupyter-notebook" data-index="9"><i class="fa fa-icon {{icon}}"></i>change cell to raw  <div title="jupyter-notebook:change-cell-to-raw" class="pull-right command-shortcut"><kbd>R</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="10"><i class="fa fa-icon {{icon}}"></i>clear all cells output  <div title="jupyter-notebook:clear-all-cells-output" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="11"><i class="fa fa-icon {{icon}}"></i>clear cell output  <div title="jupyter-notebook:clear-cell-output" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="12"><i class="fa fa-icon {{icon}}"></i>close the pager  <div title="jupyter-notebook:close-pager" class="pull-right command-shortcut"><kbd>Esc</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="15"><i class="fa fa-icon fa-repeat"></i>confirm restart kernel  <div title="jupyter-notebook:confirm-restart-kernel" class="pull-right command-shortcut"><kbd>0</kbd>,<kbd>0</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="16"><i class="fa fa-icon {{icon}}"></i>confirm restart kernel and clear output  <div title="jupyter-notebook:confirm-restart-kernel-and-clear-output" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="17"><i class="fa fa-icon fa-forward"></i>confirm restart kernel and run all cells  <div title="jupyter-notebook:confirm-restart-kernel-and-run-all-cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="18"><i class="fa fa-icon fa-repeat"></i>confirm shutdown kernel  <div title="jupyter-notebook:confirm-shutdown-kernel" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="19"><i class="fa fa-icon {{icon}}"></i>copy cell attachments  <div title="jupyter-notebook:copy-cell-attachments" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="20"><i class="fa fa-icon fa-copy"></i>copy selected cells  <div title="jupyter-notebook:copy-cell" class="pull-right command-shortcut"><kbd>C</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="22"><i class="fa fa-icon {{icon}}"></i>cut cell attachments  <div title="jupyter-notebook:cut-cell-attachments" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="23"><i class="fa fa-icon fa-cut"></i>cut selected cells  <div title="jupyter-notebook:cut-cell" class="pull-right command-shortcut"><kbd>X</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="25"><i class="fa fa-icon {{icon}}"></i>delete cells  <div title="jupyter-notebook:delete-cell" class="pull-right command-shortcut"><kbd>D</kbd>,<kbd>D</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="26"><i class="fa fa-icon {{icon}}"></i>duplicate notebook  <div title="jupyter-notebook:duplicate-notebook" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="27"><i class="fa fa-icon {{icon}}"></i>edit command mode keyboard shortcuts  <div title="jupyter-notebook:edit-command-mode-keyboard-shortcuts" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="29"><i class="fa fa-icon {{icon}}"></i>enter command mode  <div title="jupyter-notebook:enter-command-mode" class="pull-right edit-shortcut"><kbd>Esc</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="30"><i class="fa fa-icon {{icon}}"></i>enter edit mode  <div title="jupyter-notebook:enter-edit-mode" class="pull-right command-shortcut"><kbd>Enter</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="31"><i class="fa fa-icon {{icon}}"></i>extend selection above  <div title="jupyter-notebook:extend-selection-above" class="pull-right command-shortcut"><kbd>Shift-K</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="32"><i class="fa fa-icon {{icon}}"></i>extend selection below  <div title="jupyter-notebook:extend-selection-below" class="pull-right command-shortcut"><kbd>Shift-J</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="33"><i class="fa fa-icon {{icon}}"></i>find and replace  <div title="jupyter-notebook:find-and-replace" class="pull-right command-shortcut"><kbd>F</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="34"><i class="fa fa-icon {{icon}}"></i>hide all line numbers  <div title="jupyter-notebook:hide-all-line-numbers" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="35"><i class="fa fa-icon {{icon}}"></i>hide menubar  <div title="jupyter-notebook:hide-menubar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="36"><i class="fa fa-icon {{icon}}"></i>hide the header  <div title="jupyter-notebook:hide-header" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="37"><i class="fa fa-icon {{icon}}"></i>hide the toolbar  <div title="jupyter-notebook:hide-toolbar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="38"><i class="fa fa-icon {{icon}}"></i>ignore  <div title="jupyter-notebook:ignore" class="pull-right command-shortcut"><kbd>Shift</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="40"><i class="fa fa-icon {{icon}}"></i>insert cell above  <div title="jupyter-notebook:insert-cell-above" class="pull-right command-shortcut"><kbd>A</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="41"><i class="fa fa-icon fa-plus"></i>insert cell below  <div title="jupyter-notebook:insert-cell-below" class="pull-right command-shortcut"><kbd>B</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="42"><i class="fa fa-icon {{icon}}"></i>insert image  <div title="jupyter-notebook:insert-image" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="45"><i class="fa fa-icon fa-stop"></i>interrupt the kernel  <div title="jupyter-notebook:interrupt-kernel" class="pull-right command-shortcut"><kbd>I</kbd>,<kbd>I</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="46"><i class="fa fa-icon {{icon}}"></i>merge cell with next cell  <div title="jupyter-notebook:merge-cell-with-next-cell" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="47"><i class="fa fa-icon {{icon}}"></i>merge cell with previous cell  <div title="jupyter-notebook:merge-cell-with-previous-cell" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="48"><i class="fa fa-icon {{icon}}"></i>merge cells  <div title="jupyter-notebook:merge-cells" class="pull-right command-shortcut"><kbd>Shift-M</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="49"><i class="fa fa-icon {{icon}}"></i>merge selected cells  <div title="jupyter-notebook:merge-selected-cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="50"><i class="fa fa-icon fa-arrow-down"></i>move cells down  <div title="jupyter-notebook:move-cell-down" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="51"><i class="fa fa-icon fa-arrow-up"></i>move cells up  <div title="jupyter-notebook:move-cell-up" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="52"><i class="fa fa-icon {{icon}}"></i>move cursor down  <div title="jupyter-notebook:move-cursor-down" class="pull-right edit-shortcut"><kbd>Down</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="53"><i class="fa fa-icon {{icon}}"></i>move cursor up  <div title="jupyter-notebook:move-cursor-up" class="pull-right edit-shortcut"><kbd>Up</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="54"><i class="fa fa-icon {{icon}}"></i>paste cell attachments  <div title="jupyter-notebook:paste-cell-attachments" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="55"><i class="fa fa-icon {{icon}}"></i>paste cell replace  <div title="jupyter-notebook:paste-cell-replace" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="56"><i class="fa fa-icon {{icon}}"></i>paste cells above  <div title="jupyter-notebook:paste-cell-above" class="pull-right command-shortcut"><kbd>Shift-V</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="57"><i class="fa fa-icon fa-paste"></i>paste cells below  <div title="jupyter-notebook:paste-cell-below" class="pull-right command-shortcut"><kbd>V</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="62"><i class="fa fa-icon {{icon}}"></i>rename notebook  <div title="jupyter-notebook:rename-notebook" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="63"><i class="fa fa-icon {{icon}}"></i>restart kernel  <div title="jupyter-notebook:restart-kernel" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="64"><i class="fa fa-icon {{icon}}"></i>restart kernel and clear output  <div title="jupyter-notebook:restart-kernel-and-clear-output" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="65"><i class="fa fa-icon {{icon}}"></i>restart kernel and run all cells  <div title="jupyter-notebook:restart-kernel-and-run-all-cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="66"><i class="fa fa-icon {{icon}}"></i>run all cells  <div title="jupyter-notebook:run-all-cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="67"><i class="fa fa-icon {{icon}}"></i>run all cells above  <div title="jupyter-notebook:run-all-cells-above" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="68"><i class="fa fa-icon {{icon}}"></i>run all cells below  <div title="jupyter-notebook:run-all-cells-below" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="69"><i class="fa fa-icon {{icon}}"></i>run cell and insert below  <div title="jupyter-notebook:run-cell-and-insert-below" class="pull-right command-shortcut"><kbd>Alt-Enter</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="70"><i class="fa fa-icon fa-step-forward"></i>run cell and select next  <div title="jupyter-notebook:run-cell-and-select-next" class="pull-right command-shortcut"><kbd>Shift-Enter</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="71"><i class="fa fa-icon {{icon}}"></i>run selected cells  <div title="jupyter-notebook:run-cell" class="pull-right command-shortcut"><kbd>Ctrl-Enter</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="73"><i class="fa fa-icon fa-save"></i>save notebook  <div title="jupyter-notebook:save-notebook" class="pull-right command-shortcut"><kbd>Ctrl-S</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="76"><i class="fa fa-icon {{icon}}"></i>scroll cell center  <div title="jupyter-notebook:scroll-cell-center" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="77"><i class="fa fa-icon {{icon}}"></i>scroll cell top  <div title="jupyter-notebook:scroll-cell-top" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="78"><i class="fa fa-icon {{icon}}"></i>scroll notebook down  <div title="jupyter-notebook:scroll-notebook-down" class="pull-right command-shortcut"><kbd>Space</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="79"><i class="fa fa-icon {{icon}}"></i>scroll notebook up  <div title="jupyter-notebook:scroll-notebook-up" class="pull-right command-shortcut"><kbd>Shift-Space</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="80"><i class="fa fa-icon {{icon}}"></i>select next cell  <div title="jupyter-notebook:select-next-cell" class="pull-right command-shortcut"><kbd>Down</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="81"><i class="fa fa-icon {{icon}}"></i>select previous cell  <div title="jupyter-notebook:select-previous-cell" class="pull-right command-shortcut"><kbd>Up</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="83"><i class="fa fa-icon {{icon}}"></i>show all line numbers  <div title="jupyter-notebook:show-all-line-numbers" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="84"><i class="fa fa-icon fa-keyboard-o"></i>show command pallette  <div title="jupyter-notebook:show-command-palette" class="pull-right command-shortcut"><kbd>Ctrl-Shift-P</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="85"><i class="fa fa-icon {{icon}}"></i>show keyboard shortcuts  <div title="jupyter-notebook:show-keyboard-shortcuts" class="pull-right command-shortcut"><kbd>H</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="86"><i class="fa fa-icon {{icon}}"></i>show menubar  <div title="jupyter-notebook:show-menubar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="87"><i class="fa fa-icon {{icon}}"></i>show the header  <div title="jupyter-notebook:show-header" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="88"><i class="fa fa-icon {{icon}}"></i>show the toolbar  <div title="jupyter-notebook:show-toolbar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="89"><i class="fa fa-icon {{icon}}"></i>shutdown kernel  <div title="jupyter-notebook:shutdown-kernel" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="90"><i class="fa fa-icon {{icon}}"></i>shutdown kernel and close window  <div title="jupyter-notebook:close-and-halt" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="91"><i class="fa fa-icon {{icon}}"></i>split cell at cursor  <div title="jupyter-notebook:split-cell-at-cursor" class="pull-right edit-shortcut"><kbd>Ctrl-Shift-Minus</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="92"><i class="fa fa-icon {{icon}}"></i>toggle all cells output collapsed  <div title="jupyter-notebook:toggle-all-cells-output-collapsed" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="93"><i class="fa fa-icon {{icon}}"></i>toggle all cells output scrolled  <div title="jupyter-notebook:toggle-all-cells-output-scrolled" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="94"><i class="fa fa-icon fa-list-ol"></i>toggle all line numbers  <div title="jupyter-notebook:toggle-all-line-numbers" class="pull-right command-shortcut"><kbd>Shift-L</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="95"><i class="fa fa-icon {{icon}}"></i>toggle cell output  <div title="jupyter-notebook:toggle-cell-output-collapsed" class="pull-right command-shortcut"><kbd>O</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="96"><i class="fa fa-icon {{icon}}"></i>toggle cell scrolling  <div title="jupyter-notebook:toggle-cell-output-scrolled" class="pull-right command-shortcut"><kbd>Shift-O</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="98"><i class="fa fa-icon {{icon}}"></i>toggle header  <div title="jupyter-notebook:toggle-header" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="99"><i class="fa fa-icon {{icon}}"></i>toggle line numbers  <div title="jupyter-notebook:toggle-cell-line-numbers" class="pull-right command-shortcut"><kbd>L</kbd></div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="100"><i class="fa fa-icon {{icon}}"></i>toggle menubar  <div title="jupyter-notebook:toggle-menubar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="101"><i class="fa fa-icon {{icon}}"></i>toggle rtl layout  <div title="jupyter-notebook:toggle-rtl-layout" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="103"><i class="fa fa-icon {{icon}}"></i>toggle toolbar  <div title="jupyter-notebook:toggle-toolbar" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="107"><i class="fa fa-icon {{icon}}"></i>trust notebook  <div title="jupyter-notebook:trust-notebook" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="jupyter-notebook" data-index="110"><i class="fa fa-icon {{icon}}"></i>undo cell deletion  <div title="jupyter-notebook:undo-cell-deletion" class="pull-right command-shortcut"><kbd>Z</kbd></div></a></li><li class="typeahead-group" data-search-group="collapsible_headings"><a href="javascript:;">collapsible_headings command group</a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="13"><i class="fa fa-icon fa-caret-right"></i>collapse-all-headings  <div title="collapsible_headings:collapse_all_headings" class="pull-right command-shortcut"><kbd>Ctrl-Shift-Left</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="14"><i class="fa fa-icon fa-caret-right"></i>collapse-heading  <div title="collapsible_headings:collapse_heading" class="pull-right command-shortcut"><kbd>Left</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="43"><i class="fa fa-icon fa-caret-up"></i>insert-heading-above  <div title="collapsible_headings:insert_heading_above" class="pull-right command-shortcut"><kbd>Shift-A</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="44"><i class="fa fa-icon fa-caret-down"></i>insert-heading-below  <div title="collapsible_headings:insert_heading_below" class="pull-right command-shortcut"><kbd>Shift-B</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="82"><i class="fa fa-icon {{icon}}"></i>select-heading-section  <div title="collapsible_headings:select_heading_section" class="pull-right command-shortcut"><kbd>Shift-Right</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="105"><i class="fa fa-icon fa-angle-double-up"></i>toggle-collapse-all-headings  <div title="collapsible_headings:toggle_collapse_all_headings" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="106"><i class="fa fa-icon fa-angle-double-up"></i>toggle-collapse-heading  <div title="collapsible_headings:toggle_collapse_heading" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="108"><i class="fa fa-icon fa-caret-down"></i>uncollapse-all-headings  <div title="collapsible_headings:uncollapse_all_headings" class="pull-right command-shortcut"><kbd>Ctrl-Shift-Right</kbd></div></a></li><li><a href="javascript:;" data-group="collapsible_headings" data-index="109"><i class="fa fa-icon fa-caret-down"></i>uncollapse-heading  <div title="collapsible_headings:uncollapse_heading" class="pull-right command-shortcut"><kbd>Right</kbd></div></a></li><li class="typeahead-group" data-search-group="gist_it"><a href="javascript:;">gist_it command group</a></li><li><a href="javascript:;" data-group="gist_it" data-index="21"><i class="fa fa-icon fa-github"></i>create gist from notebook  <div title="gist_it:create-gist-from-notebook" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="code_font_size"><a href="javascript:;">code_font_size command group</a></li><li><a href="javascript:;" data-group="code_font_size" data-index="24"><i class="fa fa-icon fa-search-minus"></i>decrease code font size  <div title="code_font_size:decrease-code-font-size" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="code_font_size" data-index="39"><i class="fa fa-icon fa-search-plus"></i>increase code font size  <div title="code_font_size:increase-code-font-size" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="widgets"><a href="javascript:;">widgets command group</a></li><li><a href="javascript:;" data-group="widgets" data-index="28"><i class="fa fa-icon fa-sliders"></i>embed interactive widgets  <div title="widgets:embed-interactive-widgets" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="widgets" data-index="72"><i class="fa fa-icon fa-truck"></i>save clear widgets  <div title="widgets:save-clear-widgets" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="widgets" data-index="74"><i class="fa fa-icon fa-sliders"></i>save widget state  <div title="widgets:save-widget-state" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="widgets" data-index="75"><i class="fa fa-icon fa-truck"></i>save with widgets  <div title="widgets:save-with-widgets" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="code_prettify"><a href="javascript:;">code_prettify command group</a></li><li><a href="javascript:;" data-group="code_prettify" data-index="58"><i class="fa fa-icon fa-legal"></i>process-all-cells  <div title="code_prettify:process_all_cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li><a href="javascript:;" data-group="code_prettify" data-index="60"><i class="fa fa-icon fa-legal"></i>process-selected-cells  <div title="code_prettify:process_selected_cells" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="autopep8"><a href="javascript:;">autopep8 command group</a></li><li><a href="javascript:;" data-group="autopep8" data-index="59"><i class="fa fa-icon fa-legal"></i>process-all-cells  <div title="autopep8:process_all_cells" class="pull-right command-shortcut"><kbd>Ctrl-Shift-L</kbd></div></a></li><li><a href="javascript:;" data-group="autopep8" data-index="61"><i class="fa fa-icon fa-legal"></i>process-selected-cells  <div title="autopep8:process_selected_cells" class="pull-right command-shortcut"><kbd>Ctrl-L</kbd></div></a></li><li class="typeahead-group" data-search-group="auto"><a href="javascript:;">auto command group</a></li><li><a href="javascript:;" data-group="auto" data-index="97"><i class="fa fa-icon fa-comment-o"></i>toggle codefolding  <div title="auto:toggle-codefolding" class="pull-right command-shortcut"><kbd>Alt-F</kbd></div></a></li><li class="typeahead-group" data-search-group="toc2"><a href="javascript:;">toc2 command group</a></li><li><a href="javascript:;" data-group="toc2" data-index="102"><i class="fa fa-icon fa-list"></i>toggle toc  <div title="toc2:toggle-toc" class="pull-right no-shortcut">{{shortcut}}</div></a></li><li class="typeahead-group" data-search-group="varInspector"><a href="javascript:;">varInspector command group</a></li><li><a href="javascript:;" data-group="varInspector" data-index="104"><i class="fa fa-icon fa-crosshairs"></i>toggle variable inspector  <div title="varInspector:toggle-variable-inspector" class="pull-right no-shortcut">{{shortcut}}</div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 2</strong>  <div title="jupyter-notebook:change-cell-to-heading-2" class="pull-right command-shortcut"><kbd>2</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 6</strong>  <div title="jupyter-notebook:change-cell-to-heading-6" class="pull-right command-shortcut"><kbd>6</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 2</strong>  <div title="jupyter-notebook:change-cell-to-heading-2" class="pull-right command-shortcut"><kbd>2</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 3</strong>  <div title="jupyter-notebook:change-cell-to-heading-3" class="pull-right command-shortcut"><kbd>3</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 3</strong>  <div title="jupyter-notebook:change-cell-to-heading-3" class="pull-right command-shortcut"><kbd>3</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 2</strong>  <div title="jupyter-notebook:change-cell-to-heading-2" class="pull-right command-shortcut"><kbd>2</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 3</strong>  <div title="jupyter-notebook:change-cell-to-heading-3" class="pull-right command-shortcut"><kbd>3</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 5</strong>  <div title="jupyter-notebook:change-cell-to-heading-5" class="pull-right command-shortcut"><kbd>5</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 1</strong>  <div title="jupyter-notebook:change-cell-to-heading-1" class="pull-right command-shortcut"><kbd>1</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to markdown</strong>  <div title="jupyter-notebook:change-cell-to-markdown" class="pull-right command-shortcut"><kbd>M</kbd></div></a></li></ul></div></div></form></div></div></div></div><div class="modal cmd-palette" style="display: none;"><div class="modal-dialog"><div class="modal-content"><div class="modal-body"><form><div class="typeahead-container"><div class="typeahead-field"><span class="typeahead-query" style="position: relative;"><input type="search"><input type="search" readonly="readonly" unselectable="on" tabindex="-1" class="typeahead-hint" style="border-color: transparent; position: absolute; top: 0px; display: inline; z-index: -1; float: none; color: rgb(192, 192, 192); box-shadow: none; cursor: default; user-select: none;"></span><span class="typeahead-button"><button type="submit"><span class="typeahead-search-icon"></span></button></span></div><div class="typeahead-result"><ul class="typeahead-list"><li class="typeahead-group" data-search-group="jupyter-notebook"><a href="javascript:;">jupyter-notebook command group</a></li><li class="active"><a href="javascript:;" data-group="jupyter-notebook" data-index="0"><i class="fa fa-icon {{icon}}"></i><strong>change cell to heading 6</strong>  <div title="jupyter-notebook:change-cell-to-heading-6" class="pull-right command-shortcut"><kbd>6</kbd></div></a></li></ul></div></div></form></div></div></div></div></body></html>