const readline = require('readline-sync')
const fs = require('fs')

let catalogo = JSON.parse(fs.readFileSync('catalogo.json'))


const seriesFiltradasPorEstado = (opcaoDeEstadoDaSerie) => {

  let estadoDasSeries = '';
  if(opcaoDeEstadoDaSerie === 1) {
    estadoDasSeries = 'assistir';
  } else if(opcaoDeEstadoDaSerie === 2) {
    estadoDasSeries = 'assistido';
  }
  const catalogoFiltrado = catalogo.filter((serie) => serie.estado === estadoDasSeries);

  return catalogoFiltrado;
}

const listarSeriesOrdenadas = (tipoDeOrdenacao, catalogo) => {
  if(tipoDeOrdenacao === 1) {
    return catalogo.sort((serieAtual, novaSerie) => {
      if(serieAtual.nome > novaSerie.nome) {
        return 1
      } else if(serieAtual.nome < novaSerie.nome) {
        return -1
      } else {
        return 0
      }
    })
  } else if(tipoDeOrdenacao === 2) {
     return catalogo.sort((serieAtual, novaSerie) => {
        if(serieAtual.distribuidor > novaSerie.distribuidor) {
          return 1
        } else if(serieAtual.distribuidor < novaSerie.distribuidor) {
          return -1
        } else {
          return 0
        }
     })
  } else if(tipoDeOrdenacao === 3) {
     return catalogo.sort((serieAtual, novaSerie) => serieAtual.prioridade - novaSerie.prioridade)
    
  }
}

const exibirSeries = (catalogo) => {
   const seriesASeremExibidas = catalogo.map((serie) => {
      return `Serie ${serie.nome} da plataforma ${serie.distribuidor}`
  }).join('\n')

  console.log(`\n${seriesASeremExibidas}\n`)
}

let sair = false;
while(!sair) {
  const opcao = readline.questionInt(`O que você quer fazer?
  1 - Adicionar série
  2 - Listar as séries
  3 - Editar
  4 - Excluir
  0 - sair:
  > `)
  
  if(opcao === 1) {
    const novaSerieParaOCatalogo = {
      nome: '',
      distribuidor: '',
      prioridade: 0,
      estado: 'assistir'
    }
  
    const nomeDaNovaSerie = readline.question('Qual é o nome da nova série? ');
    novaSerieParaOCatalogo.nome = nomeDaNovaSerie;
  
    const nomeDoDistribuidor = readline.question('Qual é o nome da plataforma? ');
    novaSerieParaOCatalogo.distribuidor = nomeDoDistribuidor;
  
      const prioridadeDaNovaSerie = readline.questionInt('Qual é a prioridade da série? (1 à 3) ');
    novaSerieParaOCatalogo.prioridade = prioridadeDaNovaSerie;
  
    
    catalogo.push(novaSerieParaOCatalogo);
  } else if (opcao === 2) {
    const opcaoDeEstadoDaSerie = readline.questionInt(`Informe uma opção:
  1 - Séries que ainda não assistiu
  2 - Séries que já assistiu
  > `);

    const tipoDeOrdenacao = readline.questionInt(`Informe o tipo de ordenação?
  1 - Nome
  2 - Plataforma
  3 - Prioridade
  > `)

    const catalogoFiltrado = seriesFiltradasPorEstado(opcaoDeEstadoDaSerie);
    const catalogoOrdenado = listarSeriesOrdenadas(tipoDeOrdenacao, catalogoFiltrado);

    exibirSeries(catalogoOrdenado);
  } else if(opcao === 3) {
    const nomeDaNovaSerieASerAtualizada = readline.question('Qual é o nome da série que você já assistiu? ');

    const catalogoModificado = catalogo.map((serie) => {
      if(serie.nome === nomeDaNovaSerieASerAtualizada) {
        serie.estado = 'assistido';
      }

      return serie
    })

    catalogo = catalogoModificado;
  } else if (opcao === 4) {
    const nomeDaSerieASerRemovida = readline.question('Qual é o nome da série que você quer excluir? ');

    const indexDaSerieASerRemovida = catalogo.findIndex((serie) => serie.nome === nomeDaSerieASerRemovida)

    catalogo.splice(indexDaSerieASerRemovida, 1);
  } else if (opcao === 0) {
    fs.writeFileSync('catalogo.json', JSON.stringify(catalogo))
    
    sair = true;
  }
    
}[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/UFN7TVW9)
