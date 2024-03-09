<script>
    import { goto } from "$app/navigation";

    let formData = JSON.parse(localStorage.getItem("formData"));
    let isConfirmed;

    function handleConfirm() {
        // Hier könnten dann die Daten an das Backend gesendet werden
        isConfirmed = true;
    }

    function handleCancle() {
        // Gehe zurück, ohne Daten an das Backend weiter zu senden
        goto("/register");
    }
</script>


<h1>Bestätigen Sie bitte Ihre Spende:</h1>

<!-- Liste der eingetragenen Kleidungen -->
<p class="font-bold">Sie möchten folgende Kleidungsstücke spenden:</p>
<div>
    <ul>
        {#each formData.clothingList as item}
            <li>{item.quantity} x {item.name}</li>
            <hr class="w-64 h-0.5 mx-1 my-1 bg-slate-300 rounded">
        {/each}
    </ul>
</div>

<!-- Weitere Informationen -->
<div class="mt-6">
    <p class="font-bold">Die Spende soll an folgendes Krisengebiet gesendet werden:</p>
    <p class="mb-3">{formData.crisisArea}</p>
    <p class="font-bold">So erfolgt die Übergabe der Kleiderspende:</p>
    {#if formData.pickupType === "adress"} 
        <p class="mb-3">Per Abholung an Wunschadresse</p> 
    {:else} 
        <p class="mb-3">Per Übergabe an der Geschäftsstelle</p> 
    {/if}

    {#if formData.pickupType === "adress"}
        <p class="font-bold">Straße und Hausnummer für Abholung:</p> 
        <p class="mb-3">{formData.pickupStreet}</p>
        <p class="font-bold">Postleitzahl für Abholung:</p>
        <p class="mb-3">{formData.pickupZip} </p> 
        <p class="font-bold">Gewünschtes Datum für Abholung:</p>
        <p class="mb-3">{formData.date}</p> 
        <p class="font-bold">Gewünschte Uhrzeit für Abholung:</p> 
        <p class="mb-3">{formData.time}</p> 
    {/if}
</div>

<!-- Bestätigungs oder Cancle Buttons -->
{#if !isConfirmed}
    <div class="mt-6">
        <button on:click={handleConfirm} class="btn mr-2">
            Eingaben Bestätigen
        </button>

        <button on:click={handleCancle} class="btn">
            Eingabe ändern
        </button>
    </div>
{/if}

<!-- Bestätitgungstext -->
{#if isConfirmed}
    <div mt-10>
        <p1 class="text-lg font-bold text-emerald-300">Die Eingaben sind bestätigt, danke für Ihre Spende!</p1>
    </div>
{/if}