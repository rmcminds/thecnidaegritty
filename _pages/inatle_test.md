---
layout: default
title: iNatle_test
permalink: /iNatle_test/
---

<iframe id="shinyIframe" width="100%" style="border:0;"></iframe>

<!-- JavaScript to pass parameters -->
<script>
  function getQueryParams(url) {
    let params = {};
    let parser = new URL(url);
    let queryString = parser.search.slice(1);
    let pairs = queryString.split("&");

    pairs.forEach(function(pair) {
      let [key, value] = pair.split("=");
      params[key] = decodeURIComponent(value || "");
    });

    return params;
  }

  let currentUrl = window.location.href;
  let params = getQueryParams(currentUrl);
  let iframeUrl = "/iNatle_raw/index.html";

  let queryString = Object.keys(params).map(key => key + '=' + encodeURIComponent(params[key])).join('&');
  if (queryString) {
    iframeUrl += '?' + queryString;
  }

  document.getElementById('shinyIframe').src = iframeUrl;
  
  window.addEventListener('message', function(event) {
    // Debugging: Log the event data and origin
    console.log("Received message:", event.data);
    console.log("From origin:", event.origin);
    
    var iframe = document.getElementById('shinyIframe');
    iframe.style.height = event.data + 'px';
  });
</script>

<br>

<a style="text-align: center" href="https://github.com/rmcminds/iNatle/issues">Tell me about bugs or leave a suggestion!</a>