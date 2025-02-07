+++
title = "whoami"
+++

<pre>
    > neofetch
    
                        'c.          piotr@kusinski.dev
                     ,xNMM.          -------------------------------
                   .OMMMMo           OS: macOS 15.2 24C101 arm64
                   OMMM0,            Host: Mac15,9
         .;loddo:' loolloddol;.      Kernel: Monster Enjoyer 69.420
       cKMMMMMMMMMMNWMMMMMMMMMM0:    Uptime: <span id="sinceBirth"></span>
     .KMMMMMMMMMMMMMMMMMMMMMMMWd.    Packages: 113 (brew)
     XMMMMMMMMMMMMMMMMMMMMMMMX.      Shell: zsh 5.9
    ;MMMMMMMMMMMMMMMMMMMMMMMM:       Resolution: 3440x1440, 2560x1440
    :MMMMMMMMMMMMMMMMMMMMMMMM:       DE: Aqua
    .MMMMMMMMMMMMMMMMMMMMMMMMX.      WM: Rectangle
     kMMMMMMMMMMMMMMMMMMMMMMMMWd.    Terminal: iTerm2
     .XMMMMMMMMMMMMMMMMMMMMMMMMMMk   Terminal Font: HackNF-Regular 14
      .XMMMMMMMMMMMMMMMMMMMMMMMMK.   CPU: Apple M3 Max
        kMMMMMMMMMMMMMMMMMMMMMMd     GPU: Apple M3 Max
         ;KMMMMMMMWXXWMMMMMMMk.      Memory: jak złota rybka
           .cooc,.    .,coo:.
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

        /* some sanitization */
        seconds < 0 ? seconds += 60 : minutes --;
        seconds >= 0 && seconds < 10 ? seconds = "0" + seconds : seconds;
        minutes < 0 ? minutes += 60 : hours --;
        hours < 0 ? hours += 24 : days --;
        if (days < 0) {
            const lastMonth = new Date(now.getFullYear(), now.getMonth(), 0);
            days += lastMonth.getDate();
            months--;
        }
        months < 0 ? months += 12 : years--;

        return `${years-1} years, ${months} months, ${days} days, ${hours}:${minutes}:${seconds}`
    };

    let update = () => {
        document.getElementById("sinceBirth").textContent = getElapsedTime();
    };

    update();
    setInterval(update, 1000);
};

updateElapsedTime();
</script>