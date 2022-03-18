# KermitProtocol
A C implementation of the simplified Kermit file transfer protocol.


ksender sends files to kreceiver and it's checked, using bash scripts, if the files were correctly received by kreceiver.

# Despre implementare
- setarile conexiunii dintre sender si reciever sunt tinute intr-o structura definita in lib.h, numita settings
- pentru trimiterea pachetului initial 'S' membrii s (structura settings), sunt pusi in array-ul data al packetului 
- campul LEN din packete descrie numarul de bytes care chiar contin date importate, i.e. pachetul de tip 'S' va avea LEN egal cu cu numarul de bytes folosit de settings din DATA[], adica 11, la care se adauga numarul de bytes de dupa LEN din structura pkt (adica 1 pentru SEQ, TYPE si MARK si 2 pentru CHECK)
- verificarea folosind crc nu a fost implementata
- prin for-ul din "ksender.c" de la linia 53 se incearca verificarea trimiterii packetului la reciever ; daca se trimite de 3 ori si nu se primeste (adica, ksender nu primeste ACK sau NACK, atunci se intrerupe functionearea acestuia -> return 0)
- local_seq din reciever are rolul de a verificarea desincronizarea conexiunii dintre cele doua puncte ale conexiunii 
- setarile conexiunii sunt salvate si pe partea recieverului in cazul ca va fi nevoie de ele
- sett_configured are valoarea 1 daca s-a trecut de etapa configurarii setarilor
- ready_to_respond trebuie ia valoarea 1 cand s-a primit mesajul de la sender si urmeaza trimitea inapoi a unui packet de tip ACK sau NACK
- daca local_seq este 1 acest caz va fi tratat special, altfel raspunsul pentru celelalte pachete va fi tratat in aceiasi maniera
