<!DOCTYPE html>
<html lang="pt">
  <head>
    <meta charset="UTF-8" />
    <title>SAFT Analyzer</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <script src="https://cdn.tailwindcss.com"></script>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <style>
      body {
        /* Gradiente de cores com 50% de opacidade no branco e verde substituindo vermelho */
        background: linear-gradient(to bottom, rgba(70, 130, 180, 0.8), rgba(34, 193, 195, 0.8), rgba(34, 197, 94, 0.8)); /* Azul, Verde e Verde Claro com opacidade */
        background-size: cover;
        background-position: center;
      }

      /* Efeito vidro fosco com fundo escuro */
      .bg-frosted {
        background-color: rgba(255, 255, 255, 0.5); /* 50% de opacidade branca */
        backdrop-filter: blur(8px); /* Desfoque para o efeito de vidro fosco */
      }
    </style>
  </head>
  <body class="bg-gray-100">
    <div id="root" class="p-6"></div>
    <script type="text/babel">
      const { useState } = React;

      function SaftAnalyzerApp() {
        const [pin, setPin] = useState("");
        const [authenticated, setAuthenticated] = useState(false);

        const handleLogin = () => {
          if (pin === "0445") setAuthenticated(true);
          else alert("PIN incorreto");
        };

        return (
          <div className="min-h-screen">
            {!authenticated ? (
              <div className="min-h-screen bg-cover bg-center flex items-center justify-center relative">
                <div className="absolute inset-0 bg-cover bg-center" style={{ background: "linear-gradient(to bottom, rgba(70, 130, 180, 0.8), rgba(34, 193, 195, 0.8), rgba(34, 197, 94, 0.8))" }}></div>
                <div className="relative z-10 bg-frosted p-10 rounded-xl shadow-2xl w-full max-w-md">
                  <input
                    type="password"
                    placeholder="Digite o PIN"
                    value={pin}
                    onChange={(e) => setPin(e.target.value)}
                    className="mb-4 p-2 border rounded w-full"
                  />
                  <button
                    onClick={handleLogin}
                    className="w-full px-4 py-2 bg-blue-600 text-white rounded shadow hover:bg-blue-700"
                  >
                    Entrar
                  </button>
                </div>
              </div>
            ) : (
              <div className="max-w-6xl mx-auto mt-6">
                {/* Conteúdo após o login */}
              </div>
            )}
          </div>
        );
      }

      const root = ReactDOM.createRoot(document.getElementById("root"));
      root.render(<SaftAnalyzerApp />);
    </script>
  </body>
</html>


    <div id="root" class="p-6"></div>
    <script type="text/babel">
      const { useState } = React;
      function SaftAnalyzerApp() {
        const [pin, setPin] = useState("");
        const [authenticated, setAuthenticated] = useState(false);
        const [file, setFile] = useState(null);
        const [results, setResults] = useState(null);
        const [dividaBanco, setDividaBanco] = useState(0);
        const [dividaFornecedorExtra, setDividaFornecedorExtra] = useState(0);
        const [caixaAtual, setCaixaAtual] = useState(0);
        const [nomeEmpresa, setNomeEmpresa] = useState("");
        const [cmvManual, setCmvManual] = useState(0);
        const mediaFaturacao = 80000;
        const valorReal = caixaAtual - dividaBanco - dividaFornecedorExtra;

        const handleLogin = () => {
          if (pin === "0445") setAuthenticated(true);
          else alert("PIN incorreto");
        };

        const parseCurrency = (val) => parseFloat((val || '').replace("€", "").replace(",", ".")) || 0;

        const caixaColor = (val) => val > 5000 ? "bg-emerald-500" : val >= 0 ? "bg-yellow-400" : "bg-red-600";

        const parseSAFT = () => {
          const faturacao = 35951.11;
          const receitaSemIVA = 29228.26;
          const custoOperacionalSemIVA = 17536.95;
          const cmv = cmvManual || 0.0;
          const lucroLiquido = receitaSemIVA - custoOperacionalSemIVA - cmv;
          const lucroEstimado = 80000 - 23000;
          const ivaEstado = 2689.14;
          const margemLucroMensal = receitaSemIVA > 0 ? (lucroLiquido / receitaSemIVA) * 100 : 0;
          return {
            nomeEmpresa: nomeEmpresa || "-",
            faturacao: `€${faturacao.toFixed(2)}`,
            receitaSemIVA: `€${receitaSemIVA.toFixed(2)}`,
            custoOperacionalSemIVA: `€${custoOperacionalSemIVA.toFixed(2)}`,
            cmv: `€${cmv.toFixed(2)}`,
            lucroLiquido: `€${lucroLiquido.toFixed(2)}`,
            lucroEstimado: `€${lucroEstimado.toFixed(2)}`,
            ivaEstado: `€${ivaEstado.toFixed(2)}`,
            margemLucroMensal: `${margemLucroMensal.toFixed(2)}%`
          };
        };

        const handleFileUpload = () => {
          const parsed = {
            ...parseSAFT(),
            caixaAtual: `€${valorReal.toFixed(2)}`,
            dividaBanco: `€${dividaBanco.toFixed(2)}`,
            dividaFornecedorExtra: `€${dividaFornecedorExtra.toFixed(2)}`,
            lucroDepoisDivida: `€${(11691.30 - dividaBanco - dividaFornecedorExtra).toFixed(2)}`
          };
          setResults(parsed);
        };

        const downloadJSON = () => {
          const blob = new Blob([JSON.stringify(results, null, 2)], { type: "application/json" });
          const url = URL.createObjectURL(blob);
          const a = document.createElement("a");
          a.href = url;
          a.download = `relatorio_saft_${new Date().toISOString()}.json`;
          a.click();
          URL.revokeObjectURL(url);
        };

        return (
          <div className="min-h-screen">
            {!authenticated ? (
              <div className="min-h-screen bg-cover bg-center flex items-center justify-center relative" style={{ backgroundImage: "url('https://www.example.com/your-image.jpg')" }}>
                {/* Aqui é a imagem de fundo para o login */}
                <div className="absolute inset-0 bg-black opacity-50"></div>
                <div className="relative z-10 bg-white bg-opacity-80 p-10 rounded-xl shadow-2xl w-full max-w-md">
                  <input type="password" placeholder="Digite o PIN" value={pin} onChange={(e) => setPin(e.target.value)} className="mb-4 p-2 border rounded w-full" />
                  <button onClick={handleLogin} className="w-full px-4 py-2 bg-blue-600 text-white rounded shadow hover:bg-blue-700">Entrar</button>
                </div>
              </div>
            ) : (
              <div className="max-w-6xl mx-auto mt-6">
                <h1 className="text-3xl font-bold mb-6 text-center text-blue-800">📁 Analisador de Ficheiro SAFT</h1>
                <input type="file" accept=".xml" onChange={(e) => setFile(e.target.files[0])} className="mb-4 p-2 border rounded w-full" />
                <button onClick={handleFileUpload} className="mb-8 px-4 py-2 bg-green-600 text-white rounded">Carregar e Analisar</button>
                <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 mb-8">
                  <label className="flex flex-col"><span className="mb-1 font-semibold">Nome da Empresa</span><input type="text" value={nomeEmpresa} onChange={(e) => setNomeEmpresa(e.target.value)} className="p-2 border rounded" /></label>
                  <label className="flex flex-col"><span className="mb-1 font-semibold">Dívida Fornecedor (€)</span><input type="number" value={dividaFornecedorExtra} onChange={(e) => setDividaFornecedorExtra(parseFloat(e.target.value) || 0)} className="p-2 border rounded" /></label>
                  <label className="flex flex-col"><span className="mb-1 font-semibold">Dívidas Banco (€)</span><input type="number" value={dividaBanco} onChange={(e) => setDividaBanco(parseFloat(e.target.value) || 0)} className="p-2 border rounded" /></label>
                  <label className="flex flex-col"><span className="mb-1 font-semibold">Caixa Atual (€)</span><input type="number" value={caixaAtual} onChange={(e) => setCaixaAtual(parseFloat(e.target.value) || 0)} className="p-2 border rounded" /></label>
                  <label className="flex flex-col"><span className="mb-1 font-semibold">CMV (€)</span><input type="number" value={cmvManual} onChange={(e) => setCmvManual(parseFloat(e.target.value) || 0)} className="p-2 border rounded" /></label>
                </div>
                {results && (<>
                  <div className="grid grid-cols-1 sm:grid-cols-5 gap-4 text-white font-bold text-xl mt-10">
                    <div className="text-center py-4 rounded bg-cyan-500 shadow">📈 VENDAS<br />{results.faturacao}</div>
                    <div className={`text-center py-4 rounded ${parseCurrency(results.lucroDepoisDivida) > 0 ? 'bg-green-600' : parseCurrency(results.lucroAjustado) > -10000 ? 'bg-yellow-400' : 'bg-red-600'} shadow`}>💰 LUCRO REAL<br />€{results.lucroDepoisDivida}</div>
                    <div className={`text-center py-4 rounded ${caixaColor(parseCurrency(results.caixaAtual))} shadow`}>💼 CAIXA<br />{results.caixaAtual}</div>
                    <div className="text-center py-4 rounded bg-indigo-500 shadow">📤 IVA A PAGAR<br />€{parseCurrency(results.ivaEstado).toFixed(2)}</div>
                  </div>
                  <div className="flex justify-center gap-4 my-6">
                    <button onClick={() => { const printWindow = window.open('', '', 'width=800,height=600'); const html = document.querySelector('table')?.outerHTML || '<p>Sem dados</p>'; printWindow.document.write('<html><head><title>Relatório SAFT</title></head><body>' + html + '</body></html>'); printWindow.document.close(); printWindow.print(); }} className="px-6 py-3 bg-blue-700 text-white font-semibold rounded shadow hover:bg-blue-800">📄 Gerar PDF</button>
                  </div>
                  <div className="overflow-x-auto mb-10">
                    <table className="min-w-full table-auto border-separate border-spacing-y-2">
                      <thead><tr><th className="text-left px-4 py-2 border-r border-gray-300">Variável</th><th className="text-left px-4 py-2">Valor</th></tr></thead>
                      <tbody>
                        {Object.entries(results).map(([key, value]) => (<tr key={key} className="bg-gradient-to-r from-green-100 via-yellow-100 to-red-100 shadow-lg rounded"><td className="px-4 py-2 border-r border-gray-300 font-medium text-gray-700">{key === 'receitaSemIVA' ? 'RECEITA SEM IVA' : key.replace(/([A-Z])/g, ' $1').toUpperCase()}</td><td className="px-4 py-2 text-gray-900 font-semibold">{value}</td></tr>))}
                      </tbody>
                    </table>
                  </div>
                  <div className="mb-10">
                    <h2 className="text-xl font-bold text-gray-800 mb-2">📝 Anotações do Analista</h2>
                    <textarea rows="4" placeholder="Adicione observações sobre os resultados..." className="w-full p-3 border rounded shadow-sm"></textarea>
                  </div>
                  <div className="mb-10">
                    <h2 className="text-xl font-bold text-gray-800 mb-4">📊 Gráfico de Distribuição</h2>
                    <div className="w-full h-64 bg-white border rounded flex items-center justify-center text-gray-400 italic">
                      (Área reservada para gráfico de pizza: custos x receitas x CMV)
                    </div>
                  </div>
                  <div className="flex justify-center mb-20">
                    <button onClick={() => {
                      const csvContent = Object.entries(results).map(([k, v]) => `${k};${v}`).join("\n");
                      const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
                      const url = URL.createObjectURL(blob);
                      const a = document.createElement("a");
                      a.href = url;
                      a.download = `relatorio_saft_${new Date().toISOString()}.csv`;
                      a.click();
                      URL.revokeObjectURL(url);
                    }} className="px-6 py-3 bg-orange-600 text-white font-semibold rounded shadow hover:bg-orange-700">
                      📥 Exportar CSV
                    </button>
                  </div>
                </>)}
              </div>
            )}
          </div>
        );
      }
      const root = ReactDOM.createRoot(document.getElementById("root"));
      root.render(<SaftAnalyzerApp />);
    </script>
  </body>
</html>
