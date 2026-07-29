# Remover Receitas 
exemplo
\instances\Pokemao\minecraft\scripts\remover_farms.zs
craftingTable.remove(<item:easy_villager:iron_farm>);
craftingTable.remove(<item:easy_villager:wither_farm>);
https://docs.blamejared.com/1.21.1/en/fabric/api/item/FabricItemStack/

# Limitar Pasture Blocks

### 0. valores padrões em *minecraft/config/pasturelimit.json*

### 1. `/pasturelimit reload`
* **Descrição:** Recarrega o arquivo de configuração do mod a partir do disco.
* **Mensagem de retorno:** `[PastureLimit] Config reloaded.`

---

### 2. `/pasturelimit info`
* **Descrição:** Exibe as configurações atuais ativas no mod, como:
  * Limites padrão (geral e de blocos de pastagem).
  * Se o empilhamento (*stacking*) está habilitado e qual o limite máximo empilhado.
  * Se o bypass do modo criativo está bloqueado.
  * Quantidade de regras de cargos (*ranks*), limites específicos por jogador, limites por dimensão, espécies banidas, labels banidas, etc.

---

### 3. `/pasturelimit check <player>`
* **Descrição:** Verifica o limite atual de Pokémon em blocos de pastagem permitido para um jogador específico na dimensão em que ele se encontra.
* **Parâmetros:**
  * `<player>`: Nome do jogador.

---

### 4. `/pasturelimit checkblocks <player>`
* **Descrição:** Verifica a quantidade atual de blocos de pastagem que o jogador colocou no mundo em relação ao limite total dele.
* **Parâmetros:**
  * `<player>`: Nome do jogador.

---

### 5. `/pasturelimit recalc <player>`
* **Descrição:** Força a recontagem de blocos de pastagem de um jogador específico fazendo uma busca no mundo e exibe o novo valor.
* **Parâmetros:**
  * `<player>`: Nome do jogador.

---

### 6. `/pasturelimit recalcall`
* **Descrição:** Faz uma varredura completa no mundo e recalcula a contagem de blocos de pastagem de todos os jogadores.

---

### 7. `/pasturelimit setplayer <player> <limit>`
* **Descrição:** Define um limite personalizado de Pokémon em blocos de pastagem para um jogador específico (salva diretamente na configuração).
* **Parâmetros:**
  * `<player>`: Nome do jogador.
  * `<limit>`: Número inteiro entre `-1` e `9999` (onde `-1` representa ilimitado).

---

### 8. `/pasturelimit removeplayer <player>`
* **Descrição:** Remove o limite personalizado de Pokémon de um jogador, fazendo com que ele volte a seguir os limites padrão ou de rank.
* **Parâmetros:**
  * `<player>`: Nome do jogador.

---

### 9. `/pasturelimit setblocks <player> <limit>`
* **Descrição:** Define um limite personalizado de blocos de pastagem que um jogador específico pode colocar no mundo.
* **Parâmetros:**
  * `<player>`: Nome do jogador.
  * `<limit>`: Número inteiro entre `-1` e `9999` (onde `-1` representa ilimitado).

---

### 10. `/pasturelimit removeblocks <player>`
* **Descrição:** Remove o limite personalizado de blocos de pastagem de um jogador específico.
* **Parâmetros:**
  * `<player>`: Nome do jogador.

{
  // Limite padrão de Pokémon por pastagem para jogadores comuns (sem cargo)
  "default_limit": 4,

  // Se true, os limites de múltiplos cargos/ranks do LuckPerms se somam
  "allow_stacking": false,

  // O limite máximo absoluto de Pokémon por pastagem caso o empilhamento acima esteja ativado
  "max_stacked_limit": 16,

  // Se true, impede que jogadores no modo Criativo ignorem as regras de limite
  "prevent_bypass_creative": false,

  // Limites específicos aplicados apenas em certas dimensões (ex: Nether e End)
  "per_dimension_limits": {
    "minecraft:the_nether": 2,
    "minecraft:the_end": 2
  },

  // Limites de Pokémon individuais por jogador (gerado automaticamente pelo comando /pasturelimit setplayer)
  "per_player_limits": {},

  // Configurações personalizadas para cargos do LuckPerms (ranks)
  "rank_limits": [
    {
      // Nó de permissão associado ao grupo do LuckPerms
      "permission_node": "group.champion",
      // Limite de Pokémon na pastagem para este grupo
      "limit": 16,
      // Limite de blocos de pastagem que podem colocar no mundo (-1 = sem limite)
      "pasture_blocks_limit": -1,
      // Limites de tipos/labels específicas para este grupo na pastagem
      "label_limits": {
        "legendary": 3,
        "mythical": 3,
        "paradox": 4
      }
    },
    {
      "permission_node": "group.ace_trainer",
      "limit": 10,
      "pasture_blocks_limit": 16,
      "label_limits": {
        "legendary": 2,
        "mythical": 1
      }
    },
    {
      "permission_node": "group.trainer",
      "limit": 6,
      "pasture_blocks_limit": 12,
      "label_limits": {}
    },
    {
      "permission_node": "group.youngster",
      "limit": 4,
      "pasture_blocks_limit": 8,
      "label_limits": {}
    }
  ],

  // Limite padrão de blocos de pastagem que um jogador comum pode colocar no mundo
  "default_pasture_blocks_limit": 8,

  // Limites individuais de blocos por jogador (gerado pelo comando /pasturelimit setblocks)
  "per_player_block_limits": {},

  // Se true, avisa o jogador no chat quando ele tentar exceder um limite
  "notify_on_deny": true,

  // Mensagens exibidas para o jogador quando uma ação é negada (suporta códigos de cor do Minecraft com '§')
  "deny_message": "§cYour pasture is full! (%current%/%max% Pokémon)",
  "blacklist_deny_message": "§cYou cannot pasture %species%!",
  "species_limit_deny_message": "§cYou already have %current% %species% in this pasture (max: %max%)!",
  "label_limit_deny_message": "§cYou can only have %max% §e%label% §cPokémon in this pasture! (%current%/%max%)",
  "block_limit_deny_message": "§cYou have reached your pasture block limit! (%current%/%max% blocks placed)",

  // Lista de espécies de Pokémon que são proibidas de serem colocadas em qualquer pastagem
  "blacklisted_species": [
    "smeargle"
  ],

  // Lista de etiquetas (labels) que são totalmente proibidas de serem colocadas na pastagem (ex: Lendários)
  "blacklisted_labels": [
    "legendary",
    "mythical"
  ],

  // Limites de quantidade para espécies de Pokémon específicas na mesma pastagem
  "species_limits": [
    {
      "species": "ralts",
      "max": 2
    },
    {
      "species": "magikarp",
      "max": 3
    }
  ],

  // Limites globais de quantidade de Pokémon por etiquetas específicas por pastagem
  "label_limits": {
    "ultrabeast": 2,
    "paradox": 2
  }
}
### Balancear trocas
minecraft\config\cobbledollars
