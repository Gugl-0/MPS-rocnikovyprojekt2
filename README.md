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

### Třetí problém:

Poslední problém byl s asteroidy:

- Spawnuli se v borderu a ten potom smazali

  To jsem vyřešil tak že jsem zmenšil pole kde se asteroidy můžou spawnovat

- Pokud jsem asteroid sestřelil objevilo se misto něj X, to se stát mělo ale X mělo po chvilce zmizetcoč se nestalo a X tam zůstalo
  Tohle jsem vyřešil tím, že 100ms jsem místo "X" vypsal prázdno " "

Pak už jsem žádný problém nenašel

## Hra

Cílem hry je dosáhnout 100 score aniž by mi asteroidy zničili stíhačku

Score ze získavá sestřelováním asteroidů. Každý zničený asteroid = +10 score

Po hracím poli se hýbe pomocí šipek a stříli se pomocí mezerníku

Stíhačka má 3 životy a na každý život má 5 energie. Pokud příjdu o pět energie (trefí mě 5 asteroidů) příjdu o jeden život

## Co jsem upravil

Na kódu jsem upravil pár věcí které se mi nelíbily

### Pomalejší asteroidy

1-Na můj vkus padaly asteroidy až moc rychle tak jsem je zpomalil

**To jsem udelal takhle:**

Asteroidy se posouvaly a spawnovali každý snímek.

Přidal jsem do main loopu proměnou "frameCount" která počítá počet snímku, pak jsem přidal so loopu který vykresloval asteroidy to že má vykreslit další asteroidy když je počet snímků dělitelný 4 se zbytkem 0 (frameCount % 4 = 0)

Takhle se budou posouvat a spawnovat asteroidy každý 4. snímek, může se to ale jednoduše změnit a to tak že v "frameCount % 4 = 0" vyměním 4 za jiné číslo a to podle toho po kolika snímcích chci posunout a sapwnout asteroidy

2-Konce hry se dosáhlo moc rychle

Tak jsem změníl to že pokud sestřelím asteroid tak se mi do score přičte 5 míto 10

Taky jsem zvýšil cílove score z 100 na 150

3-Jako energie se zobrazoval znak "Ů" to jsem změníl na znak 176 v ascii kódu "░"

- Chtěl jsem ještě přidat limit nábojů kdy když mi dojdou náboje tak bude game over jenže to se mi asi po hodině zkoušení (i s botem) nedařilo tak jsem to vzdal

# Videa














