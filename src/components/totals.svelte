<script>
  import { v_list } from "../stores.js";

  let v_curr;

  const unsubscribe = v_list.subscribe(value => {
    v_curr = value;
  });

  let tot_stealth = 0;
  let tot_resistance = 0;
  let tot_stagespeed = 0;
  let tot_transmission = 0;

  let cureList = [
    "Copper, Silver, Iodine, Iron, Carbon",
    "Potassium, Ethanol, Lithium, Silicon, Bromine",
    "Table salt, Sugar, Orange juice, Tomato juice, Milk",
    "Spaceacillin, Saline-glucose solution, Epinephrine, Multiver",
    "Oil, Synaptizine, Mannitol, Space drugs, Cryptobiolin",
    "Phenol, Inacusiate, Oculine, Antihol",
    "Leporazine, Mindbreaker toxin, Corazone",
    "Pax, Happiness, Ephedrine",
    "Lipolicide, Salicyclic acid",
    "Haloperidol, Aranesp, Diphenhydramine",
    "Modafinil, Anacea"
  ];
  let chemicalList = [
    "Virus Rations",
    "Virus Food",
    "Mutagenic Agar",
    "Sucrose Agar",
    "Weakened Virus Plasma",
    "Virus Plasma",
    "Unstable Uranium Gel",
    "Stable Uranium Gel"
  ];
  let infectionList = ["Airborne", "Touch", "Fluid", "Blood"];

  let cure = cureList[0];
  let chemical = [];
  let infection = infectionList[3];

  $: {
    tot_stealth = 0;
    tot_resistance = 0;
    tot_stagespeed = 0;
    tot_transmission = 0;
    cure = cureList[0];
    chemical = [];
    infection = infectionList[3];
    v_curr.forEach(virus => {
      tot_stealth += virus.stealth;
      tot_resistance += virus.resistance;
      tot_stagespeed += virus.stagespeed;
      tot_transmission += virus.transmission;

      switch (virus.level) {
        case 1:
          check_unique(chemicalList[0]);
          break;
        case 2:
          check_unique(chemicalList[1]);
          break;
        case 3:
          check_unique(chemicalList[2]);          
          break;
        case 4:
          check_unique(chemicalList[3]);
          break;
        case 5:
          check_unique(chemicalList[4]);
          break;
        case 6:
          check_unique(chemicalList[5]);          
          break;
        case 7:
          check_unique(chemicalList[6]);
          break;
        case 8:
          check_unique(chemicalList[7]);
          break;
      }
    });
    switch (tot_resistance) {
      case 1:
        cure = cureList[0];
        break;
      case 2:
        cure = cureList[1];
        break;
      case 3:
        cure = cureList[2];
        break;
      case 4:
        cure = cureList[3];
        break;
      case 5:
        cure = cureList[4];
        break;
      case 6:
        cure = cureList[5];
        break;
      case 7:
        cure = cureList[6];
        break;
      case 8:
        cure = cureList[7];
        break;
      case 9:
        cure = cureList[8];
        break;
      default:
        if (tot_resistance < 1) {
          cure = cureList[0];
        } else {
          cure = cureList[8];
        }
    }
    if (tot_transmission < 3) {
      infection = infectionList[3];
    }
    if (tot_transmission >= 3 && tot_transmission < 7) {
      infection = infectionList[2];
    }
    if (tot_transmission >= 7 && tot_transmission < 11) {
      infection = infectionList[1];
    }
    if (tot_transmission >= 11) {
      infection = infectionList[0];
    }
  }

  function check_unique(chem_item) {
    if (!chemical.includes(chem_item)) {
      chemical.push(chem_item);
    }
  }
</script>

<div class="totals">
  <table class="table">
    <tr>
      <td>
        <b>Stealth</b>
      </td>
      <td>{tot_stealth}</td>
    </tr>
    <tr>
      <td>
        <b>Resistance</b>
      </td>
      <td>{tot_resistance}</td>
    </tr>
    <tr>
      <td>
        <b>Stage speed</b>
      </td>
      <td>{tot_stagespeed}</td>
    </tr>
    <tr>
      <td>
        <b>Transmission</b>
      </td>
      <td>{tot_transmission}</td>
    </tr>
    <tr>
      <td>Chemicals required</td>
      <td>
        {#each chemical as chemical_item}
          <p>{chemical_item}</p>
        {/each}
      </td>
    </tr>
    <tr>
      <td>Possible cures</td>
      <td>{cure}</td>
    </tr>
    <tr>
      <td>Infection type</td>
      <td>{infection}</td>
    </tr>
  </table>
</div>
