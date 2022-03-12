## Pipeline GitOps com [FluxCD](https://fluxcd.io/)

### Objetivo:
Irá auxiliar na implantação, atualização das aplicações no cluster kubernetes de forma automatizada.

### O que é GitOps?
A ideia central do GitOps é que exista uma “fonte da verdade” um repositório git que contém arquivos declarativos da infraestrutura (arquivos declarativos são os manifestos yaml que serão utilizados no deploy da aplicação no cluster). E um processo automatizado irá garantir que tudo que está no git estará em execução no cluster Kubernetes.

### O que Flux?
O Flux é uma solução de entrega contínua e progressiva para Kubernete, ele mantém o cluster Kubernete em sincronia com a fonte da verdade (repositórios Git) e automatiza as implantações e atualizações.
#### Conseitos fundamentais:
- **Fonte da verdade:** É um repositório contendo o estado desejado da nossa aplicação. As alterações são verificadas em um intervalo de tempo definido, se houver uma versão mais recente um novo artefato será reproduzido. 
- **Reconciliação:** garante o estado desejado definido declarativamente na fonte da verdade. 
- **Bootstrap:** O processo de instalação dos componentes do Flux.

### Infraestrutura:
![FluxCD](./img/infra.png)

### Detalhes do pipeline:
1. O desenvolvedor commita as mudanças no repositório da aplicação.
2. Github Actions irá buildar a imagem da aplicação e envia para repo docker, utilizaremos os 8 caracteres do hash do commit. 
3. Ainda no repositório da aplicação serão colhidas algumas váriaveis de ambiente para preencher os manifestos que serão utilizados no k8s. 
4. Após o preenchimento dos manifestos o mesmo passará por uma validação usaremos kubeval.
5. Após a validação do manifesto será aberto um PR automaticamente no repositório do FluxCD.
6. Após as aprovações do PR e feito o merge fluxcd identifica as mudanças e faz o deploy no cluster. 