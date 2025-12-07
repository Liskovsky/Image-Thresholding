# Segmentace a Prahování Obrazu

Tento semestrální projekt se zaměřuje na implementaci technik **segmentace a prahování obrazu** pro rozdělení digitálních snímků na různé oblasti zájmu.

---

## Segmentace Obrazu

**Segmentace obrazu** je technika rozdělování obrazu do dílčích oblastí nebo odlišných objektů. Míra podrobnosti, do které je dělení provedeno, závisí na řešeném problému. Segmentace by se měla zastavit, když byly detekovány objekty nebo oblasti zájmu v aplikaci. Segmentace netriviálních snímků je jedním z nejobtížnějších úkolů v oblasti digitálního zpracování obrazu.

## Prahování (Thresholding)

**Prahování** je nejjednodušší metoda pro segmentaci obrazu. Pomocí prahování je pixelům přiřazena jedna ze dvou hodnot na základě porovnání jejich intenzity s určitou prahovou hodnotou (threshold).

### Globální prahování

**Globální prahování** používá jednu prahovou hodnotu pro celý obrázek.

#### Algoritmus (Globální prahování)

1.  Zvolíme počáteční prahovou hodnotu $T$ (např. průměrnou hodnotu intenzity celého obrázku).
2.  Segmentujeme obrázek na dvě skupiny pixelů: $G_1$ (pixely s intenzitou $> T$) a $G_2$ (pixely s intenzitou $\le T$).
3.  Vypočítáme novou prahovou hodnotu $T_{nový}$ jako průměr průměrných intenzit pixelů ve skupinách $G_1$ a $G_2$.
4.  Pokud je rozdíl $|T - T_{nový}|$ menší než malá hodnota $\Delta T$, algoritmus se zastaví.
5.  Jinak nastavíme $T = T_{nový}$ a opakujeme krok 2.

### Variabilní prahování

**Variabilní prahování** je složitější technika, která se používá, pokud je osvětlení obrázku nerovnoměrné, nebo pokud se vlastnosti objektu liší po celém snímku. Tato metoda vypočítává různé prahové hodnoty pro různé oblasti obrázku.

---

## Praktická Část (Implementace)

Projekt obsahuje aplikaci, která demonstruje kroky potřebné pro segmentaci obrázku.

### 1. Načtení obrázku

Uživatel vybere obrázek, který bude dále analyzován.

### 2. Zobrazení histogramu

Po načtení obrázku je zobrazen jeho **histogram**, který poskytuje vizuální reprezentaci distribuce intenzit pixelů.

### 3. Segmentace obrázku

Segmentace se provádí na základě zadané prahové hodnoty.

* **Vstup:** Prahová hodnota $T$.
* **Transformace:** Každý pixel $P(x, y)$ je porovnán s $T$:
    * Pokud $P(x, y) > T$, je výstupní pixel nastaven na **bílou** (255).
    * Pokud $P(x, y) \le T$, je výstupní pixel nastaven na **černou** (0).

### 4. Zobrazení a uložení

Výsledný **segmentovaný obrázek** je zobrazen v aplikaci a uživatel má možnost ho uložit do souboru (JPEG, PNG).

### Kód k aplikaci (Hlavní třídy)

Aplikace je postavena na třídě `ImageSegmentationApp`, která zahrnuje metody pro načtení, zobrazení a uložení obrázku.

```python
# Úsek kódu demonstrující uložení obrázku
def save_image(self):
    if self.segmented_image_label and self.image_path:
        path = filedialog.asksaveasfilename(defaultextension=".jpg", filetypes=[("JPEG Image", "*.jpg"),
                                                                                 ("PNG Image", "*.png"), ("All Files", "*.*")])
        if path:
            segmented_image = self.threshold_segmentation()
            if segmented_image is not None:
                cv2.imwrite(path, segmented_image)
                print("Image saved successfully.")
            else:
                messagebox.showerror("Error", "Failed to save image.")

def main():
    root = tk.Tk()
    app = ImageSegmentationApp(root)
    root.mainloop()

if __name__ == '__main__':
    main()
