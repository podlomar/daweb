---
title: Výběr data cesty
demand: 2
---

Na konci tohoto cvičení bude uživatel schopen vybrat datum cesty podle dat stažených z API. Budeme postupovat obdobně, jako s komponentou `CityOptions`. Tentokrát však vytvoříme komponentu `DatesOptions`, která vygeneruje elementy `option` do výběru termínů cesty. Termíny cest se budou získávat z API
endpointu [/api/dates](https://leviexpress-backend.herokuapp.com/api/dates). Prohlédněte si výstup API a všimněte si, že tentokrát v datech nevrací pole objektů, ale pole stringů.

1. Komponentu `DatesOptions` vytvořte opět přímo v souboru s komponentami `JourneyPicker` a `CityOptions`.
1. HTML kód s elementy `<option>` pro výběr termínu přesuňte ze `select`u pro výběr data do komponenty `DatesOptions`. V `select`u použijte vytvořenou komponentu `DatesOptions`. Zkontrolujte v prohlížeči, že se výběr termínů zobrazuje stále stejně.
1. Podobně jako `CityOptions` získává seznam měst v property `cities`, bude i `DatesOptions` získávat seznam termínů v property `dates`. Elementy `<option>` (s výjimkou prvního ručně vloženého s textem „Vyberte“) v této komponentě nebudou mít atribut `value`. Místo toho se jako hodnota použije přímo text (datum).
1. Připravte si pomocí `useState` další stav `dates`. Pro otestování si do něj vložte data pro tento víkend: `['22. května 2021', '23. května 2021']`. Použijte stav `dates` pro naplnění hodnot property `dates` tam, kde je použita komponenta `DatesOptions`. Ověřte v prohlížeči, že se ve výběru termínů zobrazují dvě uvedená data.
1. Upravte `useEffect` volaný při prvním zobrazení komponenty. Vedle seznamu měst bude z API získávat také seznam termínů. Endpoint je na adrese
   ```
   https://leviexpress-backend.herokuapp.com/api/dates
   ```
   a vrací seznam termínů ve formátu, který máme připraven. Změňte výchozí stav `dates` na prázdné pole a po té do něj nastavte výsledek volání uvedeného
   endpointu.
1. Ověřte v prohlížeči, že se do `select`ů načítají data (města a termíny) a že po kliknutí na tlačítko „Vyhledat spoj“ se uživatelem zvolené údaje vypíší do konzole prohlížeče.
1. Commitněte změny.
