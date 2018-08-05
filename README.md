This repository contains a proof-of-concept implementation of [Operating Channel Validation (OCV)](https://papers.mathyvanhoef.com/wisec2018.pdf).

Note that the branch `local_development` will have its history rewritten while the patches are being improved. The idea is that the final set of commits will be included on the official repository of `hostap`. This means that if you base your own code or project on the branch `local_development`, do not expect to easily merge new updates, as this will likely lead to merge conflicts.

# Configuring Operating Channel Validation (OCV)

To enable OCV for a client, add the line `ocv=1` to a network block. Note that to use OCV you must also enable Management Frame Protection (MFP) using the `ieee80211w=1` line. For example, the following configuration has OCV enabled:

    network={
      ssid="testnetwork"
      psk="abcdefgh"
      pairwise=CCMP
      group=CCMP
      ieee80211w=1
      ocv=1
    }

To enable for an AP, add the line `ocv=` to the configuration. Again you must also enable MFP using `ieee80211w=1`. An example configuration is

    interface=wlan1
    ssid=testnetwork
    channel=6

    wpa=2
    wpa_passphrase=abcdefgh
    rsn_pairwise=CCMP
    group_cipher=CCMP

    ieee80211w=1
    ocv=1

# Interoperability and Correctness

A station (i.e. client or AP) that has OCV enabled is still able to connect to a station that does not support OCV. We assure this by running a bunch of automated tests. These tests also assure that OCV is working properly when enabled. To run all the tests yourself, read the documentation file [tests/hwsim/README](tests/hwsim/README) to prepare the automated tests, and then execute the following command in `tests/hwsim`:

    ./start.sh
    ./run-tests.py -f ocv
    ./stop.sh

This will run all tests related to our new OCV functionality.
