<script>
// id dos elementos (ajuste se forem diferentes)
const btnDefinir = document.getElementById('btn-definir-link'); // "Definir link da planilha"
const btnReload  = document.getElementById('btn-recarregar');   // "Recarregar"

function getSheetUrl() {
  return localStorage.getItem('sheet_csv_url') || '';
}

function setSheetUrl(url) {
  localStorage.setItem('sheet_csv_url', url.trim());
}

async function carregarCSV() {
  const url = getSheetUrl();
  const erroEl = document.getElementById('erro'); // <span id="erro"></span>
  erroEl.textContent = '';
  if (!url) {
    erroEl.textContent = 'Defina o link CSV da planilha.';
    return;
  }
  try {
    const resp = await fetch(url, { mode: 'cors' });
    if (!resp.ok) throw new Error(`HTTP ${resp.status}`);
    const csvText = await resp.text();
    // TODO: parseie com PapaParse ou similar e atualize o painel
    // const data = Papa.parse(csvText, { header: true }).data;
    console.log('CSV carregado:', csvText.slice(0, 200));
  } catch (e) {
    erroEl.textContent = `Erro ao carregar CSV: ${e.message}`;
  }
}

btnDefinir?.addEventListener('click', () => {
  const atual = getSheetUrl();
  const novo = prompt('Cole o link CSV público da planilha:', atual);
  if (novo) {
    setSheetUrl(novo);
    carregarCSV();
  }
});

btnReload?.addEventListener('click', carregarCSV);

// carrega automaticamente ao abrir
document.addEventListener('DOMContentLoaded', carregarCSV);
</script>
