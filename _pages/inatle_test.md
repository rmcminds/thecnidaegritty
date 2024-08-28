---
layout: default
title: iNatle_test
permalink: /iNatle_test/
---

<iframe id="shinyIframe" width="100%" style="border:0;"></iframe>

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

  function resizeIframeToContentSize(iframe) {
    const iframeDocument = iframe.contentDocument || iframe.contentWindow.document;
    const container = iframeDocument.getElementById('mycontainer'); // Target the specific container
    if (container) {
      iframe.style.height = container.scrollHeight + 'px'; // Set iframe height based on the container's scrollHeight
    }
  }

  document.getElementById('shinyIframe').onload = function() {
    // Adjust the iframe height on initial load
    resizeIframeToContentSize(this);

    // Watch for changes in the iframe content
    const frameElement = this;
    let lastScrollHeight = frameElement.contentWindow.document.getElementById('mycontainer').scrollHeight;
    let watcher;

    const watch = () => {
      cancelAnimationFrame(watcher);

      const container = frameElement.contentWindow.document.getElementById('mycontainer'); // Access the specific container
      if (lastScrollHeight !== container.scrollHeight) {
        resizeIframeToContentSize(frameElement);
      }
      lastScrollHeight = container.scrollHeight;
      watcher = requestAnimationFrame(watch); // Continuously check for changes
    };

    watcher = requestAnimationFrame(watch); // Start the watcher
</script>

<br>

<a style="text-align: center" href="https://github.com/rmcminds/iNatle/issues">Tell me about bugs or leave a suggestion!</a>
