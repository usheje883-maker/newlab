yeah standalone build of the studio hosted at https://aya.nodium.lol/

i wont give any support for this, you have an old webserver that include the basics to get it running.
it wont work on github pages or stuff don't cry in my discord if you can't configure it up (it shouldn't be that complicated).

the threaded wasm build needs SharedArrayBuffer, so the page has to be cross-origin isolated:
open it on localhost or over https, and send

    Cross-Origin-Opener-Policy: same-origin
    Cross-Origin-Embedder-Policy: require-corp

on the html (the included webserver already does). if you open it on http://<lan-ip>:8000 or on a
plain http:// domain the page is not a secure context, so the engine stops early and tells you
instead of booting into "SharedArrayBuffer transfer requires self.crossOriginIsolated".

on a static host that cannot send headers (github pages etc) coi-serviceworker.js adds them from a
service worker, so keep that file next to index.html, allow service workers + site data, and let the
page reload itself once. a hard reload (ctrl+shift+r) bypasses the worker and lands you back on the
error message.
