## 📄 HackerText Component

### Descrizione
`HackerText` mostra un testo con effetto "hacker":  
1. Prima mostra uno sfondo colorato (fase bg, tipo blocchi █).
2. Poi un breve effetto glitch.
3. Infine il testo appare come se venisse digitato, carattere per carattere.

L’effetto parte solo quando il componente entra in view e la prop `active` è `true`.  
Può essere usato in sequenza per effetti a catena.

---

### Props

| Nome       | Tipo           | Default    | Descrizione                                                                 |
|------------|----------------|------------|-----------------------------------------------------------------------------|
| `text`     | `string`       | **(req)**  | Il testo da visualizzare (supporta anche multilinea con `\n`).              |
| `className`| `string`       | `""`       | Classi CSS aggiuntive.                                                      |
| `color`    | `string`       | `#00FF41`  | Colore del testo, del glitch e dello sfondo fase bg.                        |
| `speed`    | `number`       | `30`       | Velocità in ms per carattere durante la fase typing.                        |
| `active`   | `boolean`      | `true`     | Se `true`, parte l’effetto. Se `false`, resta in fase bg.                   |
| `onDone`   | `() => void`   |            | Callback chiamata quando l’effetto è completato (utile per sequenze).       |
| `onReset`  | `() => void`   |            | Callback chiamata quando il testo esce dalla view (per resettare la sequenza).|

---

### Esempio di utilizzo base

```tsx
<HackerText text="ACCESS GRANTED" color="#00FF41" />
```

---

### Esempio di utilizzo in sequenza

Per far partire più `HackerText` uno dopo l’altro, usa uno stato array e le callback `onDone`/`onReset`:

```tsx
const [ready, setReady] = useState([true, false, false]);

const handleDone = (idx: number) => {
  setReady(prev => {
    const next = [...prev];
    if (idx + 1 < next.length) next[idx + 1] = true;
    return next;
  });
};

const handleReset = (idx: number) => {
  setReady(prev => {
    const next = [...prev];
    for (let i = idx; i < next.length; i++) next[i] = false;
    if (idx === 0) next[0] = true;
    return next;
  });
};

<HackerText
  text="Primo testo"
  active={ready[0]}
  onDone={() => handleDone(0)}
  onReset={() => handleReset(0)}
/>
<HackerText
  text="Secondo testo"
  active={ready[1]}
  onDone={() => handleDone(1)}
  onReset={() => handleReset(1)}
/>
<HackerText
  text="Terzo testo"
  active={ready[2]}
  onDone={() => handleDone(2)}
  onReset={() => handleReset(2)}
/>
```

---

### Note
- Se usi più `HackerText` in sequenza, **devi** gestire lo stato come sopra.
- Se vuoi solo l’effetto su un testo singolo, basta la prop `text`.
- Il componente mostra sempre la fase bg quando non attivo o fuori view.

---