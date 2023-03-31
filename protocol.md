# Protocollo Briscola (SBGP)
## Simple Briscola Game Protocol

Il protocollo si basa sull'invio di datagram di lunghezza fissa di 2 byte.
Il datagram è così compostosto:
```
+-------+-------------+---------------+
| o o o | x x x x x x | y y y y y y y |
+-------+-------------+---------------+
```
Dove:
- `o` sono i 3 bit che definiscono le operazioni possibili.
- `x` sono i 6 bit che contengono i dati delle carte.
- `y` sono i 7 bit sono riservati per informazioni ed eventuali estensioni del protocollo.

Operazioni:

I/O | id | nome      | parametri                  | note
:--:|:--:|:---------:|:--------------------------:|:---------------------------------------------
X   | 0  | `ACK`     |                            |	in risposta ai messaggi
O   | 1  | `INIT`    | `id_carta first id_player` | avvio della partita `id_carta` indica la briscola
O   | 2  | `GIV_C`   | `id_carta`				  |	dà una carta al giocatore
O   | 3  | `ENM_C`   | `id_carta`                 |	il nemico ha giocato una certa carta
O   | 4  | `END_G`   | `id_player`	              |	partita terminata con `id_player` vincente
I   | 5  | `SEARCH`  | `id_player`                |	informa il server di voler giocare
I   | 6  | `USE_C`   | `id_carta`                 |	utente gioca la carta
X   | 7  | `ERR`     | `err_id`                   |

# `id_carta`
```
+-----+---------+
| s s | n n n n |
+-----+---------+
```
`s` indica il sengo
id | segno       
:-:|:-------:
 0 | denari
 1 | spade
 2 | coppe
 3 | bastoni

`n` indica il numero
id | valore
:-:|:-------------------:
 0 | riservato ad indicare il segno
 1 | asso
 2 | due
...|...
 7 | sette
 8 | fante
 9 | cavallo
 10 | re

# `id_err`
Non ancora implementata

# Procedura di gioco di una partita
- giocatoi G1 invia un segnale di `SEARCH` al server.
- quando trova un giocatore esso invia un `INIT` con un identificativo dell'altro giocatore.
- il server invia per tre volte `GIV_C`.
- il giocatore gioca la carta con `USE_C` e riceverà la carta del nemico con `ENM_C` (o vice versa se parte secondo).
- alla terminazione della partita il server invia `END_G` col vincitore.
