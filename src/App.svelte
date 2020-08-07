<script>
  import { onMount } from "svelte";
  import * as Papa from "papaparse";
  import { saveAs } from "file-saver";
  import FileInput from "./Fileinput.svelte";

  async function parseFile({ detail }) {
    const { file } = detail;
    const data = await getData(file);

    // remove meta lines
    console.log(data.data);
    data.data = data.data.slice(3);
    data.data = data.data.slice(0, -2);

    // create new obj
    const newData = [["Date", "Payee", "Memo", "Outflow", "Inflow"]];
    data.data.forEach(el => {
      const row = [el[0], "", el[5], el[6], el[7]];
      newData.push(row);
    });
    // data.data = data.data.slice(-2);
    const parsedCsv = Papa.unparse(newData);
    var blob = new Blob([parsedCsv], { type: "text/plain;charset=utf-8" });
    saveAs(blob, "csv.csv");
  }

  async function getData(file) {
    return new Promise(resolve => {
      Papa.parse(file, {
        complete: function(results) {
          resolve(results);
        }
      });
    });
  }
</script>

<main>
  <FileInput on:input={parseFile} />
</main>
