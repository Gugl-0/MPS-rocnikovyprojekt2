# MPS-rocnikovyprojekt2


Jako projekt na první pololetí jsem udělal hru Snake, a protože mě to bavilo tak jsem se rozhodl udělat znovu nějakou hru.

Tentokrát jsem si vybral něco jako Space Shooter.

Space shooter to přímo není ale je to na podobném principu, mám stíhačku se kterou mužu střílet a nic me nesmí trefit.

Hru jsem opět dělal podle tutoriálu:

https://kickingbrains.wordpress.com/2016/08/03/space-shooter-a-simple-c-game-for-windows/

Kód měl pár problému ktere jsem musel vyřešit

### První problém

Když jsem hru chtěl spustit tak se nic nestalo. V kódu byl totiž špatně použitý getch() a kbhit().

Funkce getch() slouží k získání znaku z klávesnice bez čekání na enter (jako třeba u scanf_s) ale také ji nevypíšr na obrazovku

Funkce kbhit() má podobnou funkci a to takovou že čeká na jakýkoli input, pokud žadný input nepřichází vrací hodnotu "false" a pokud ano vrací hodnotu "true". Běží na pozadí takže kód na nic nečeká

Problém byl v tom že getch() a kbhit() jsou pro starší verze c++. Novější c++ používá _getch() a _lbhit() takže vyřešit problém bylo lehký

### Druhý problém

Pokaždé když jsem program spustil, po chvíli mi spadl a hodil mi chybu  _STL_VERIFY(this->_Ptr != _Mycont->_Myhead, "cannot increment end list iterator");

S tímhle jsem nevěděl co dělat tak jsem použil bota.

Problém byl v tom že jsem v jednom loopu zároveň inkrementoval proměnnou "bullet" ale zároveň ji mazal

**Bot mi poradil že mám:**

    for (bullet = Bullets.begin(); bullet != Bullets.end(); bullet++) {
        (*bullet)->Move();
        if ((*bullet)->isOut()) {
            delete(*bullet); 
            bullet = Bullets.erase(bullet);
        }
    }

**Vyměnit za:**

    for (bullet = Bullets.begin(); bullet != Bullets.end(); ) {
        (*bullet)->Move();
        if ((*bullet)->isOut()) {
            delete(*bullet); 
            bullet = Bullets.erase(bullet); // erase vrací nový platný iterator
        } else {
            ++bullet;
        }
    }





