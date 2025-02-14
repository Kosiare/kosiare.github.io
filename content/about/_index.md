+++
title = "whoami"
+++

<style>
    .hide-content {
        display: none;    
    }

    .show-content {
        display: block;
    }
</style>

<div id="animated-terminal" class="termynal show-content" data-ty-startDelay="100" data-ty-typeDelay="110" data-termynal>
    <span data-ty="input" data-ty-delay="500" data-ty-prompt="~/piotr/kusinski.dev/about >"> whoami</span>
    <span data-ty data-ty-delay="100">Fetching data...</span>
    <span data-ty="progress" data-ty-progressLength="50"></span>
    <span data-ty data-ty-delay="100"></span>
    <span data-ty data-ty-delay="30">##############+-++++########################</span>
    <span data-ty data-ty-delay="30">###########---.--+-. ...####################</span>
    <span data-ty data-ty-delay="30">##########-.-----...    .-##################    piotr@kusinski.dev</span>
    <span data-ty data-ty-delay="30">#########-...-..-----+++ ..#################    -------------------------------</span>
    <span data-ty data-ty-delay="30">#########..-.-------####+. -################    OS: Arch btw</span>
    <span data-ty data-ty-delay="30">#########. ---+++#######+. -################    Host: Kubernetes Cluster</span>
    <span data-ty data-ty-delay="30">######### -+  .###+. -+##- .################    Kernel: Hardened (until I forget an update)</span>
    <span data-ty data-ty-delay="30">######### -+-.. +#-..-+##+ -################    Uptime: <span id="sinceBirth"></span></span>
    <span data-ty data-ty-delay="30">######### ++--+-###+++###+ +################    Packages: ∞ (pods)</span>
    <span data-ty data-ty-delay="30">########+.++##++#########++#-###############    Shell: bash/zsh</span>
    <span data-ty data-ty-delay="30">#########.-+++- .+##########################    SLA: 99.99999% (occasional naps)</span>
    <span data-ty data-ty-delay="30">#########+-+++-.--#######++#################    Status: Permanently online</span>
    <span data-ty data-ty-delay="30">##########+--+++++######+     +#############    On-Call Status: PagerDuty PTSD enabled</span>
    <span data-ty data-ty-delay="30">###########+++########+-#        .##########    Logs: Filtered 10 million lines, still no clue</span>
    <span data-ty data-ty-delay="30">######### .  +....-.. -##         ##########    Monitoring: Prometheus, Grafana & occasional panic</span>
    <span data-ty data-ty-delay="30">#######+ .   ++-   -+####-        +#########    Coffee: Black, no sugar</span>
    <span data-ty data-ty-delay="30">#######++     +++++#####.          #########    CPU: Multi-threaded, but bottlenecked by meetings</span>
    <span data-ty data-ty-delay="30">#######++     ---+###               ########    Memory: ∞ (with occasional leaks)</span>
    <span data-ty data-ty-delay="30">#######+       -++.                   ######    </span>
    <span data-ty data-ty-delay="30">###+-                                    ###    </span>
    <span data-ty data-ty-delay="100"></span>
    <span data-ty="input" data-ty-delay="3600000" data-ty-prompt="~/piotr/kusinski.dev/about >"></span>
</div>

<pre id="static-terminal" class="neofetch hide-content">
> whoami

##############+-++++########################
###########---.--+-. ...####################    
##########-.-----...    .-##################    piotr@kusinski.dev
#########-...-..-----+++ ..#################    -------------------------------
#########..-.-------####+. -################    OS: Arch btw
#########. ---+++#######+. -################    Host: Kubernetes Cluster
######### -+  .###+. -+##- .################    Kernel: Hardened (until I forget an update)
######### -+-.. +#-..-+##+ -################    Uptime: <span id="sinceBirth"></span>
######### ++--+-###+++###+ +################    Packages: ∞ (pods)
########+.++##++#########++#-###############    Shell: bash/zsh
#########.-+++- .+##########################    SLA: 99.99999% (occasional naps)
#########+-+++-.--#######++#################    Status: Permanently online
##########+--+++++######+     +#############    On-Call Status: PagerDuty PTSD enabled
###########+++########+-#        .##########    Logs: Filtered 10 million lines, still no clue
######### .  +....-.. -##         ##########    Monitoring: Prometheus, Grafana & occasional panic
#######+ .   ++-   -+####-        +#########    Coffee: Black, no sugar
#######++     +++++#####.          #########    CPU: Multi-threaded, but still bottlenecked by meetings
#######++     ---+###               ########    Memory: ∞ (with occasional leaks)
#######+       -++.                   ######    
###+-                                    ###    

</pre>

<script>
let updateElapsedTime = () => {
    const birthDate = new Date("1997-02-13T09:30:00.000Z");

    let getElapsedTime = () => {
        const now = new Date();
        let years = now.getFullYear() - birthDate.getFullYear();
        let months = now.getMonth() - birthDate.getMonth();
        let days = now.getDate() - birthDate.getDate();
        let hours = now.getHours() - birthDate.getHours();
        let minutes = now.getMinutes() - birthDate.getMinutes();
        let seconds = now.getSeconds() - birthDate.getSeconds();

        /* Adjust for negative values */
        if (seconds < 0) {
            seconds += 60;
            minutes--;
        }
        if (minutes < 0) {
            minutes += 60;
            hours--;
        }
        if (hours < 0) {
            hours += 24;
            days--;
        }
        if (days < 0) {
            const lastMonth = new Date(now.getFullYear(), now.getMonth(), 0);
            days += lastMonth.getDate();
            months--;
        }
        if (months < 0) {
            months += 12;
            years--;
        }

        /* Ensure two-digit formatting */
        const formatTime = (num) => (num < 10 ? "0" + num : num);

        return `${years} years, ${months} months, ${days} days, ${formatTime(hours)}:${formatTime(minutes)}:${formatTime(seconds)}`;
    };

    let update = () => {
        document.getElementById("sinceBirth").textContent = getElapsedTime();
    };

    update();
    setInterval(update, 1000);
};

updateElapsedTime();
</script>
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
