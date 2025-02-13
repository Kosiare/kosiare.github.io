+++
title = "whoami"
+++


<link rel="stylesheet" href="termynal.css">
<script src="termynal.js"></script>
<script>
  document.addEventListener("DOMContentLoaded", () => {
    new Termynal(document.querySelector(".termynal"));
  });
</script>

<div class="termynal" data-termynal>
  <span data-ty="input">echo "Hello, World!"</span>
  <span data-ty>Output: Hello, World!</span>
  <!-- Add more command and output pairs as needed -->
</div>


<pre class="neofetch">
> neofetch


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
