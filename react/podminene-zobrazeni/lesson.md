Ve vanilla JavaScriptu jsem se nejdříve učili vytvářet statický obsah stránky a později jsme postupně přidávali interaktivitu. V Reactu budeme postupovat stejně. V této lekci uděláme další krok k tomu, aby naše stránky mohly být dynamičtější. Bude se nám k tomu hodit jedna hezká pomůcka z čistého JavaScriptu, kterou jsme v předešlých lekcích vynechali.

## Ternární operátor

Představte si situaci, kdy chceme uživateli zobrazit jednoduchou zprávu podle jeho věku.

```js
let message = null;
if (age >= 18) {
  message = 'Smíš vstoupit';
} else {
  message = 'Utíkej za mamkou';
}
```

Tato podmínka vypadá velmi přímočaře. Má však určité nevýhody.

1. Jde o celkem dlouhý kód pro velmi jednoduchou věc.
1. Musíme požívat proměnnou `let`, čemuž se snažíme co nejvíce vyhýbat.

JavaScript nám pro tuto situaci nabízí zkraktu, které se říká <term cs="operátor pro podmíněný výraz" en="conditional operator">.

```js
const message = age >= 18 ? 'Smíš vstoupit' : 'Utíkej za mamkou';
```

Tento operátor se dá použít vždy, když chceme do nějaké proměnné uložit různé hodnoty na základně nějaké podmínky.

V Reactu se nám tento operátor bude hodit ve více situacích. První z nich je situace, kdy chceme zkonstruovat název CSS třídy podle nějaké podmínky. Vezměme položku nákupního seznamu napsanou jako React komponentu.

```js
const ShoppingItem = (props) => {
  return (
    <div className="item">
      <span className="item__name">{props.name}</span>
      <span className="item__amount">{props.amount}</span>
    </div>
  );
};
```

Dejme tomu, že chceme být schopni položku vysvítit jako vybranou. Můžeme tedy komponentě přidat novou `prop` s názvem `selected` a použít ji takto.

```js
<ShoppingList name="jablka" amount="1 kg" selected={true} />
```

Budeme pak mít CSS třídu `item--selected`, která například nastavuje jinou barvu pozadí. Vybraná položka by tak měla mít atribut `className` nastaven takto.

```js
<div className="item item--selected">
```

Obsah atributu `className` tedy chceme zkonstruovat dle hodnoty `props.selected`. To bychom mohli udělat pomocí podmínky.

```js
const ShoppingItem = (props) => {
  let itemClass = null;
  if (props.selected) {
    itemClass = 'item item--selected';
  } else {
    itemClass = 'item';
  }

  return (
    <div className={itemClass}>
      <span className="item__name">{props.name}</span>
      <span className="item__amount">{props.amount}</span>
    </div>
  );
};
```

Díky podmíněnému operátoru si situaci můžeme zjednodušit takto.

```js
const ShoppingItem = (props) => {
  const itemClass = props.selected ? 'item item--selected' : 'item';

  return (
    <div className={itemClass}>
      <span className="item__name">{props.name}</span>
      <span className="item__amount">{props.amount}</span>
    </div>
  );
};
```

Dokonce bychom hodnotu ani nemuseli ukládat do proměnné a použít podmíněný operátor přímo na místě.

```js
const ShoppingItem = (props) => {
  return (
    <div className={props.selected ? 'item item--selected' : 'item'}>
      <span className="item__name">{props.name}</span>
      <span className="item__amount">{props.amount}</span>
    </div>
  );
};
```

Takovýto kód už však může být hůře čitelný, takže je dobré jej používat s mírou a uvážením.

## Předávání hodnot pomocí props

Všimněte si, jak jsme použili naši komponentu `ShoppingItem` a jak jsme jí předali prop `selected`.

```js
<ShoppingItem name="jablka" amount="1 kg" selected={true} />
```

Kdybychom to udělali takto

```js
<ShoppingItem name="jablka" amount="1 kg" selected="true" />
```

ve vlastnosti `props.selected` uvnitř komponenty bychom měli řetězec `'true'`. Pokud totiž předáváme hodnoty pro props pomocí uvozovek jako jsme zvyklí z HTML, uvnitř komponenty vždy obdržíme tyto hodnoty jako řetězce. Pokud chceme skutečnou boolean hodnotu `true` nebo `false`, musíme si pomocí složených závorek otevřít JavaScriptové okénko. Toto platí i pro ostatní hodnoty. Pokud chceme předat pomocí props číslo, provedeme to opět pomocí složených závorek.

```js
<ShoppingItem name="jablka" amount={3} selected="true" />
```

Malinko nepřehledná situace nastane, pokud takto předáváme objekty, protože pak máme vedle sebe dvě složené závorky, kde každá znamená něco jiného.

```js
<ShoppingItem name="jablka" amount={{ value: 3, unit: 'kg' }} selected="true" />
```

Vnější pár složených závorek otvírá JavaScript okénko uvnitř JSX a vnitřní pár vytváří objekt. Tento styl zápisu potkáme v druhé části lekce, kdy budeme nastavovat naším komponentám CSS styly.

### Předávání hodnoty true

Poslední trik, kterým si ulehčujeme práci možná znáte již z HTML. Pokud chceme nějaké prop nastavit hodnotu `true` jako v tomto kódu

```js
<ShoppingItem name="jablka" amount="1 kg" selected={true} />
```

stačí napsat pouze název dané prop a React už si domyslí, že do ní chceme vložit hodnotu `true`. Můžeme pak psát prostě

```js
<ShoppingItem name="jablka" amount="1 kg" selected />
```

[[[ excs Cvičení: Podmíněné výrazy
- podminene-jednohubky
- react-zarovka
]]]

## Stylování JSX elementů

V základech HTML a CSS jste se učili, že není dobré stylovat HTML elementy pomocí atributu `style`. Místo toho se vždy snažíme vytvořit nějakou dobře pojmenovanou CSS třídu. Stejné pravidlo platí i v Reactu. Stejně jako v čistém HTML však občas narazíme na výjimky. 

Vzpomeňte si například na náš projekt Café Lóra, kde jsme stylovali barvy jednotlivých ingrediencí v nápoji. Vzhledem k tomu, že kódy barev byly součástí dat stažených z API, nemohli jsme mít dopředu připravené třídy se všemi možnými barvami. V takovém případě je stylování pomocí atributu `style` oprávněné.

V JSX ke změne individuálních stylů použijeme prop jménem `style`. Obsahem této prop musí být JavaScriptový objekt, kde jednotlivé CSS vlastnosti mají jména v camel case formátu. Takto například v Reactu nastavíme barvu pozadí, barvu písma a výšku elementu.

```js
<div style={{ height: '5rem', color: 'white', backgroundColor: '#ff0000' }} />
```

Zde narážíme na situaci, kdy do nějaké prop předáváme objekt. Je proto třeba dát si dobrý pozor na složené závorky. Takovýto kód nám fungovat nebude.

```js
<div style={ height: '5rem', color: 'white', backgroundColor: '#ff0000' } />
```

Druhý důležitý fakt je, že všechny hodnoty pro naše styly musí být řetězce. Proto nám rozhodně nebude fungovat například takovýto kód.

```js
<div style={{ height: 5rem, color: white, backgroundColor: #ff0000 }} />
```

Naopak co si snadno dovolit můžeme, je nastavit hodnotu stylu pomocí podmíněného operátoru.

```js
<div
  style={{
    height: '5rem',
    color: 'white',
    backgroundColor: podminka ? '#ff0000' : '#0000ff',
  }}
/>
```

Na konci této části opět zdůrazněme, že stylování pomocí atributu `style` děláme pouze ve výjimečných a odůvodněných případech. 

## Podmíněné zobrazení.

Do této chvíle jsme na základě vstupních props komponenty měnili pouze její stylování. Často však narazíme na situaci, kdy chceme změnit i samotný obsah komponenty. Představme si, že máme například komponentu `Product`, která umožňuje vložit produkt v e-shopu do košíku.

```js
const Product = (props) => {
  return (
    <div className="product">
      <div className="product__name">{props.name}</div>
      <img className="product__img" src={props.img} />
      <button>Vložit do košíku</button>
    </div>
  );
};
```

Pokud už je však produkt objednaný, chtěli bychom zobrazit tlačítko s jiným nápisem. K tomuto můžeme s výhodou použít náš oblíbený podmíněný operátor.

<!-- prettier-ignore -->
```js
const Product = (props) => {
  return (
    <div className="product">
      <div className="product__name">{props.name}</div>
      <img className="product__img" src={props.img} />
      {
        props.ordered ? <button>Zrušit objednávku</button> : <button>Vložit do košíku</button>
      }
    </div>
  );
};
```

Všimněte si, že na místě, kde chceme provést rozhodnutí, otevřeme pomocí složených závorek JavaScriptové okno, abychom mohli použít náš podmíněný operátor. Ten pak dle hodnoty uložné v prop s názvem `ordered` vrátí příslušný kus JSX.

Pokud v podmíněném operátoru pracujeme s obsáhlejším JSX, často se nám celý výraz nevejde na jeden řádek. Pak je ve zvyku formátovat kód komponenty takto.

```js
const Product = (props) => {
  return (
    <div className="product">
      <div className="product__name">{props.name}</div>
      <img className="product__img" src={props.img} />
      {props.ordered ? (
        <button>Zrušit objednávku</button>
      ) : (
        <button>Vložit do košíku</button>
      )}
    </div>
  );
};
```

Občas se nám stane, že v jednom z případů podmínky nechcem zobrazit nic. Dejme tomu, že nechceme uživateli umožnit zrušit objednávku přímo u daného produktu. V takovém případě můžeme vrátit hodnotu `null`, která pro React znamená, že nemá zobrazit nic.

```js
const Product = (props) => {
  return (
    <div className="product">
      <div className="product__name">{props.name}</div>
      <img className="product__img" src={props.img} />
      {props.ordered ? null : <button>Vložit do košíku</button>}
    </div>
  );
};
```

[[[ excs Cvičení: Podmíněné zobrazování
- mejlik-schranka
- mejlik-hlavicka
]]]
