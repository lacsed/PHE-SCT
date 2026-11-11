# PHE-SCT — partially homomorphic supervisory control on microcontrollers

Code of the paper *Partially Homomorphic Supervisory Control of Discrete Event
Systems on Microcontrollers*, by A. C. P. Gonçalves, L. V. R. Alves and
P. N. Pena (LACSED, Universidade Federal de Minas Gerais).

A monolithic supervisor of a discrete event system runs on two ESP32-S3
microcontrollers without its state ever being decrypted. The state is a vector
of EC-ElGamal ciphertexts, one per state of the supervisor: an event moves
ciphertexts, and each control decision is a homomorphic sum followed by a test
of whether it encrypts zero. The private key is split between a supervisor
board, which holds the tables and the encrypted state, and a plant board, which
observes the events, sends each to the supervisor board encrypted, and applies
the decisions, so that neither can decrypt alone. The supervisor board signs a
record of every step, which an auditor can check afterwards. The version
described in the paper, in which the supervisor board never sees the event, is
at commit `760003a`.

| folder | contents |
|---|---|
| [`des_split/`](des_split/README.md) | the Arduino sketch of both boards, the auditor `audit.py`, `make_keys.py` for the identity keys, the host test `host_main.cpp`, and the supervisor of the two-cluster linear cluster tool |
| `notebook/` | `generator_ultrades.ipynb`, which synthesizes a supervisor with [UltraDES](https://github.com/lacsed/UltraDES) and writes its header |

## Requirements

* two ESP32-S3 boards on the same 2.4 GHz Wi-Fi network, and the Arduino-ESP32
  core (3.3.12 in the paper), whose bundled mbedTLS provides the curves;
* Python 3, standard library only, for `make_keys.py` and `audit.py`;
* for the host test, g++ with C++17 (and `libmbedtls-dev` for the real curves).

[`des_split/README.md`](des_split/README.md) explains the protocol, how to make
`secrets.h`, flash the two boards, run the host test and audit a recorded run.
`secrets.h` holds the Wi-Fi password and the identity keys and is never
committed.
