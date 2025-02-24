+++
title = "Coming soon"
type = "page"
author = "Kosiare"
+++

<div id="animated-terminal" class="termynal show-content" data-ty-startDelay="100" data-ty-typeDelay="80" data-termynal>
    <span data-ty="input" data-ty-delay="500" data-ty-prompt="~/piotr/kusinski.dev >">ls -la posts/</span>
    <span data-ty data-ty-delay="30"></span>
    <span data-ty data-ty-delay="0">total 0</span>
    <span data-ty data-ty-delay="0">drwxr-xr-x@  2 kosiare  staff    64 <span class="current-day"></span> <span class="current-month"></span> <span class="current-time"></span> .</span>
    <span data-ty data-ty-delay="0">drwxr-x---+ 65 kosiare  staff  2080 <span class="current-day"></span> <span class="current-month"></span> <span class="current-time"></span> ..</span>
    <span data-ty data-ty-delay="500"></span>
    <span data-ty="input" data-ty-delay="300" data-ty-typeDelay="80" data-ty-prompt="~/piotr/kusinski.dev >"> Coming soon...</span>
    <span data-ty="input" data-ty-delay="30000" data-ty-typeDelay="0" data-ty-prompt="~/piotr/kusinski.dev >"></span>
</div>


<script>
  document.addEventListener("DOMContentLoaded", function () {
    const now = new Date();

    const monthNames = ["jan", "feb", "mar", "apr", "may", "jun", "jul", "aug", "sep", "oct", "nov", "dec"];
    const month = monthNames[now.getMonth()];
    const day = String(now.getDate()).padStart(2, '0');
    const hours = String(now.getHours()).padStart(2, '0');
    const minutes = String(now.getMinutes()).padStart(2, '0');
    const time = `${hours}:${minutes}`;

    document.querySelectorAll(".current-month").forEach(el => el.textContent = month);
    document.querySelectorAll(".current-day").forEach(el => el.textContent = day);
    document.querySelectorAll(".current-time").forEach(el => el.textContent = time);
  });
</script>
