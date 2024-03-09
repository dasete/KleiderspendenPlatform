<script>
    import { goto } from "$app/navigation";
    
    let formData = {
        clothingList: [],
        pickupType: "",
        crisisArea: "",
        pickupStreet: "",
        pickupZip: "",
        date: "",
        time: "",
    };

    let predefinedClothingTypes = [
        { name: "T-Shirt", quantity: 0 },
        { name: "Pullover", quantity: 0 },
        { name: "Jacke", quantity: 0 },
        { name: "Hose", quantity: 0 },
        { name: "Schuhe", quantity: 0 },
        ];

    let newClothingTypes = [{ name: "", quantity: 0 }];

    let pickupZipAlert;
    let clothingListAlert;
    let pickupTypeAlert;
    let crisisAreaAlert;
    let pickupStreetAlert;
    let dateAlert;
    let timeAlert;
    
    function checkLastInput() {
      const lastItem = newClothingTypes[newClothingTypes.length - 1];
      if (lastItem.name.trim() !== "") {
        newClothingTypes = [...newClothingTypes, { name: "", quantity: 0 }];
      }
    }
  
    function generateClothingList() {
        for (let item of predefinedClothingTypes) {
            if (item.name.trim() !== "" && item.quantity > 0) {
                formData.clothingList = [...formData.clothingList, item];
            }
        }

        for (let item of newClothingTypes) {
            if (item.name.trim() !== "" && item.quantity > 0) {
                formData.clothingList = [...formData.clothingList, item];
            }
        }
    }

    function resetAlerts() {
        pickupZipAlert = false;
        clothingListAlert = false;
        pickupTypeAlert = false;
        crisisAreaAlert = false;
        pickupStreetAlert = false;
        dateAlert = false;
        timeAlert = false;
    }
    
    function isZipValid() {
        if ((formData.pickupZip.startsWith("10") && formData.pickupZip.length === 5 && formData.pickupZip.match(/\d/g).length === 5)) {
            return true; // es handelt sich um eine gültige PLZ
        }
        return false; // es handelt sich um keine gültige PLZ
    }

    function areInputsValid() {
        if (formData.clothingList.length === 0) {
            clothingListAlert = true;
        }
        if (formData.crisisArea.length === 0) {
            crisisAreaAlert = true;
        }
        if (formData.pickupType.length === 0) {
            pickupTypeAlert = true;
        }
        if (formData.pickupType === "adress") {

            if (formData.pickupStreet.length === 0) {
                pickupStreetAlert = true;
            }

            if (!isZipValid()) {
                pickupZipAlert = true;
            }

            if (formData.date.length === 0) {
                dateAlert = true;
            }

            if (formData.time.length === 0) {
                timeAlert = true;
            }
        }
        if (clothingListAlert || pickupTypeAlert || crisisAreaAlert || pickupStreetAlert || dateAlert || timeAlert || pickupZipAlert ) {
            return false;
        }
        return true;
    }

    function handleSubmit() {
        resetAlerts();
        generateClothingList();
        // Validiere, ob in allen Inputfield etwas eingetragen wurde und PLZ korrekt ist
        if (!areInputsValid()) {
            // Es fehlen Inputs oder PLZ ist falsch, daher return
            formData.clothingList = [];
            return;
        }

        // Daten lokal in Browser speichern und zu Zusammenfassung navigieren
        localStorage.setItem("formData", JSON.stringify(formData));
        goto("/summary");
    };
</script>


<div class="mx-auto">
    <h1>Bitte tragen Sie die Informationen zu Ihrer Kleiderspende ein.</h1>

    <form on:submit|preventDefault={handleSubmit}>
        <!-- Art der Kleidung -->
        <div class="mb-4">
            <label for="#">
                {#if !clothingListAlert}
                    <span class="block text-sm font-semibold mb-1">Tragen Sie bitte ein, welche Art von Kleidung und wie viele Kleidungsstücke Sie spenden möchten:</span>
                {:else}
                    <span class="block text-sm font-semibold mb-1 text-red-500">Tragen Sie bitte ein, welche Art von Kleidung und wie viele Kleidungsstücke Sie spenden möchten: | Bitte treffen Sie eine Auswahl.</span>
                {/if}
            </label>

            <div class="flex flex-col">
                <!-- Auswahlfeld für vordefinierte Kleidungstypen -->
                {#each predefinedClothingTypes as item}
                    <div class="flex items-center p-1">
                        <span  class="p-1 w-48">{item.name}</span>
                        <input class="p-1 rounded w-14 bg-slate-600 " type="number" bind:value={item.quantity} min="0" />
                    </div>
                    <hr class="w-64 h-0.5 mx-1 my-1 bg-slate-300 rounded">
                {/each}
                
                <!-- Eingabefelder für weitere Kleidungstypen -->
                {#each newClothingTypes as item, index}
                    {#if item.name.trim() !== "" || index === newClothingTypes.length - 1} <!-- Eingabefelder Löschen wenn es leer ist, und es nicht das erste feld ist -->
                        <div class="flex items-center p-1">
                            <input class="p-1 w-44 rounded mr-4 bg-slate-600" type="text" bind:value={item.name} placeholder="Anderer Kleidungstyp" on:input={checkLastInput} />
                            <input class="p-1 rounded w-14 bg-slate-600" type="number" bind:value={item.quantity} min="0" />
                        </div>
                        <hr class="w-64 h-0.5 mx-1 my-1 bg-slate-300 rounded">
                    {/if}
                {/each}
            </div>
        </div>

        <!-- Krisengebiet -->
        <div class="mb-4">
            <label for="crisisArea">
                {#if !crisisAreaAlert}
                    <span class="block text-sm font-semibold mb-1">An welches Kriesengebiet möchten Sie die Spende schicken?</span>
                {:else}
                    <span class="block text-sm font-semibold mb-1 text-red-500">An welches Kriesengebiet möchten Sie die Spende schicken? | Bitte treffen Sie eine Auswahl.</span>
                {/if} 
            </label>
            <select bind:value={formData.crisisArea} class="input" id="crisisArea">
                <option value="Krisengebiet A">Krisengebiet A</option>
                <option value="Krisengebiet B">Krisengebiet B</option>
                <option value="Krisengebiet C">Krisengebiet C</option>
                <option value="Krisengebiet D">Krisengebiet D</option>
                <option value="Krisengebiet E">Krisengebiet E</option>
            </select>
        </div>

        <!-- Abholen lassen oder Abgeben an Geschäftsstelle -->
        <div class="mb-4">
            <label for="pickupType">
                {#if !pickupTypeAlert}
                    <span class="block text-sm font-semibold mb-1">Möchten Sie die Kleidung an der Geschäftsstelle abgeben oder abholen lassen?</span>
                {:else}
                    <span class="block text-sm font-semibold mb-1 text-red-500">Möchten Sie die Kleidung an der Geschäftsstelle abgeben oder abholen lassen? | Bitte treffen Sie eine Auswahl.</span>
                {/if}
            </label>
            <select bind:value={formData.pickupType} class="input" id="isAdressPickup">
                <option value="store">Übergabe an der Geschäftsstelle</option>
                <option value="adress">Abholung</option>
            </select> 
        </div>

        <!-- Ggf Abholadresse, -datum und -uhrzeit eingeben -->
        {#if formData.pickupType === "adress"}
            <h2>Bitte geben Sie die Informationen zum gewünschten Abholort und Abholzeitpunkt ein.</h2>
            <!-- Straße und Hausnummer -->
            <div class="mb-4">
                <label for="pickupStreet">
                    {#if !pickupStreetAlert}
                        <span class="block text-sm font-semibold mb-1">Straße und Hausnummer:</span>
                    {:else}
                        <span class="block text-sm font-semibold mb-1 text-red-500">Straße und Hausnummer: | Bitte eine gültige Adresse eingeben.</span>
                    {/if}
                </label>
                <input type="text" bind:value={formData.pickupStreet} class="input" id="pickupStreet" />
            </div>

            <!-- PLZ -->
            <div class="mb-4">
                <label for="pickupZip">
                    {#if !pickupZipAlert}
                        <span class="block text-sm font-semibold mb-1">Postleitzahl:</span>
                    {:else}
                        <span class="block text-sm font-semibold mb-1 text-red-500">Postleitzahl: | Die Abholung kann nur in der Nähe der Geschäftsstelle (PLZ: 10XXX) erfolgen. Tragen Sie eine gültige PLZ ein.</span>
                    {/if}
                </label>
                <input type="text" bind:value={formData.pickupZip} class="input" id="pickupZip" />
            </div>
            
            <!-- Datum -->
            <div class="mb-4">
                <label for="date">
                    {#if !dateAlert}
                        <span class="block text-sm font-semibold mb-1">Gewünschtes Datum:</span>
                    {:else}
                        <span class="block text-sm font-semibold mb-1 text-red-500">Gewünschtes Datum: | Bitte geben Sie das gewünschte Abholdatum ein.</span>
                    {/if}
                </label>
                <input type="date" bind:value={formData.date} class="input" id="date" />
            </div>

            <!-- Uhrzeit -->
            <div class="mb-4">
                <label for="time">
                    {#if !timeAlert}
                        <span class="block text-sm font-semibold mb-1">Gewünschte Uhrzeit:</span>
                    {:else}
                        <span class="block text-sm font-semibold mb-1 text-red-500">Gewünschte Uhrzeit: | Bitte geben Sie die gewünschte Abholzeit ein.</span>
                    {/if}
                </label>
                <input type="time" bind:value={formData.time} class="input" id="time" />
            </div>
        {/if}

        <button type="submit" class="btn mt-1">Kleiderspende registrieren</button>
    </form>
</div>
