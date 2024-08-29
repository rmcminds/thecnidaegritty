---
layout: default
title: iNatle
permalink: /iNatle/
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

  let outerIframe = document.getElementById('shinyIframe');

  function resizeIframeToContentSize(iframe) {
    const iframeDocument = iframe.contentDocument || iframe.contentWindow.document;

    // Find the nested iframe inside the outer iframe
    const nestedIframe = iframeDocument.querySelector('iframe'); // Finds the first iframe within the outer iframe

    if (nestedIframe) {
      const nestedIframeDocument = nestedIframe.contentDocument || nestedIframe.contentWindow.document;
      
      // Find the largest content element within the nested iframe
      const container = nestedIframeDocument.getElementById('mycontainer');
      
      if (container) {
        iframe.style.height = Number(container.scrollHeight)+Number(5) + 'px';
      }
    } else {
      // Fallback to resizing based on the outer iframe's own content
      const container = iframeDocument.body;
      if (container) {
        iframe.style.height = Number(container.scrollHeight)+Number(5) + 'px';
      }
    }
  }

  outerIframe.onload = function() {
    resizeIframeToContentSize(outerIframe);

    const frameElement = outerIframe;
    let lastScrollHeight = frameElement.contentWindow.document.body.scrollHeight;
    let watcher;

    const watch = () => {
      cancelAnimationFrame(watcher);

      // Re-target the nested iframe on every loop
      const nestedIframe = frameElement.contentWindow.document.querySelector('iframe');
      
      if (nestedIframe) {
        const container = nestedIframe.contentWindow.document.getElementById('mycontainer');
        if (container && lastScrollHeight !== container.scrollHeight) {
          resizeIframeToContentSize(frameElement);
          lastScrollHeight = container.scrollHeight;
        }
      } else {
        const container = frameElement.contentWindow.document.body;
        if (lastScrollHeight !== container.scrollHeight) {
          resizeIframeToContentSize(frameElement);
          lastScrollHeight = container.scrollHeight;
        }
      }

      watcher = requestAnimationFrame(watch); 
    };

    watcher = requestAnimationFrame(watch);
  };
</script>

{::nomarkdown}
<p style="text-align: center">
<br>
<a style="text-align: center" href="https://github.com/rmcminds/iNatle/issues">Tell me about bugs or leave a suggestion!</a>
</p>
{:/nomarkdown}
