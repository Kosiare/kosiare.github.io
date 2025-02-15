+++
title = "contact"
+++

<style>
    .hide-content {
        display: none;    
    }

    .show-content {
        display: block;
    }
</style>

<div id="animated-terminal" class="termynal show-content" data-ty-startDelay="100" data-ty-typeDelay="80" data-termynal>
    <span data-ty="input" data-ty-delay="500" data-ty-prompt="~/piotr/kusinski.dev >">ssh contact@kusinski.dev</span>
    <span data-ty data-ty-delay="10"></span>
    <span data-ty data-ty-delay="0">Connecting to kusinski.dev...</span>
    <span data-ty="progress" data-ty-progressChar="." data-ty-progressLength="8"></span>
    <span data-ty data-ty-delay="200"></span>
    <span data-ty data-ty-delay="30"><b>Welcome to AlphaCTI [Contact Terminal Interface]</b></span>
    <span data-ty>Type 'help' to see available commands</span>
    <span data-ty data-ty-delay="10"></span>
    <span data-ty="input" data-ty-delay="700" data-ty-typeDelay="80" data-ty-prompt="$">help</span>
    <span data-ty data-ty-delay="10"></span>
    <span data-ty data-ty-delay="10"><b>Available commands:</b></span>
    <span data-ty data-ty-delay="0">- email      -> Show contact email</span>
    <span data-ty data-ty-delay="0">- social     -> Show social links</span>
    <span data-ty data-ty-delay="0">- exit       -> Disconnect</span>
    <span data-ty data-ty-delay="0"><i>More commands soon!</i></span>
    <span data-ty data-ty-delay="0"></span>
    <span data-ty="input" data-ty-delay="700" data-ty-typeDelay="80" data-ty-prompt="$">email</span>
    <span data-ty data-ty-delay="0"></span>
    <span data-ty data-ty-delay="10"><b>- <a href="mailto:piotr@kusinski.dev?subject=Hello!&body=Sup?">piotr@kusinski.dev</a></b></span>
    <span data-ty data-ty-delay="0"></span>
    <span data-ty="input" data-ty-delay="700" data-ty-typeDelay="80" data-ty-prompt="$">social</span>
    <span data-ty data-ty-delay="0"></span>
    <span data-ty data-ty-delay="10"><b>- Github:</b> <a href="https://github.com/Kosiare" target="_blank">Kosiare</a></span>
    <span data-ty data-ty-delay="0"><b>- LinkedIn:</b> <a href="https://www.linkedin.com/in/pkusinski/" target="_blank">Piotr Kusinski</a></span>
    <span data-ty data-ty-delay="0"><b>- Website:</b> <a href="https://kusinski.dev" target="_blank">kusinski.dev</a> - but you're already here</span>
    <span data-ty data-ty-delay="100"></span>
    <span data-ty="input" data-ty-delay="300" data-ty-typeDelay="80" data-ty-prompt="$">exit</span>
    <span data-ty data-ty-delay="10">Connection closed.</span>
    <span data-ty data-ty-delay="0"></span>
</div>

<pre id="static-terminal" class="neofetch hide-content">
<b>Email:</b>
<b>-</b> <a href="mailto:piotr@kusinski.dev?subject=Hello!&body=Sup?">piotr@kusinski.dev</a></b>

<b>Socials:</b>
<b>- Github:</b> <a href="https://github.com/Kosiare" target="_blank">Kosiare</a>
<b>- LinkedIn:</b> <a href="https://www.linkedin.com/in/pkusinski/" target="_blank">Piotr Kusinski</a>
<b>- Website:</b> <a href="https://kusinski.dev" target="_blank">kusinski.dev</a> - but you're already here

</pre>

<script>
    function isMobileDevice() {
        return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
    }

    let staticTerminal = document.getElementById("static-terminal");
    let animatedTerminal = document.getElementById("animated-terminal");

    if (isMobileDevice()) {
        animatedTerminal.classList.remove("show-content");
        animatedTerminal.classList.add("hide-content");
        staticTerminal.classList.add("show-content");
        staticTerminal.classList.remove("hide-content");
    }
</script>
