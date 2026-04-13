#!/bin/bash

# ==============================================
# 🇧🇷 ATITUDE BOX - O EMULADOR BRASILEIRO 🇧🇷
#      CÓDIGO MÃE - VERSÃO COMPLETA
# ==============================================

clear
echo "=============================================="
echo "          🔥 BEM-VINDO AO ATITUDE BOX        "
echo "             Versão Brasileira 🇧🇷           "
echo "=============================================="
echo ""
echo "🔄 Preparando o sistema..."
sleep 2

rm -rf ~/x
echo ""

# ==============================================
# ✨ A MAGIA ACONTECE AQUI ✨
# ==============================================
echo "📂 Solicitando permissão de armazenamento..."
echo "👉 VAI APARECER UMA JANELA AGORA!"
echo "   CLIQUE EM PERMITIR / ALLOW!"
echo ""

# ESSE COMANDO ABRE A JANELA DO SISTEMA IGUAL NO ORIGINAL
termux-setup-storage

sleep 3

# Verifica se deu certo
if [ -d ~/storage/shared ]; then
  echo "✅ Permissão concedida com sucesso!"
else
  echo "❌ Erro: Permissão não foi concedida!"
  echo "   Rode o script novamente e clique em PERMITIR."
  exit 1
fi

echo ""
echo "📦 Instalando componentes base..."

# Atualiza pacotes
echo "🔄 Atualizando lista de pacotes..."
apt-get clean
apt-get update -y >/dev/null 2>&1
apt-get upgrade -y >/dev/null 2>&1

# Instala pacotes essenciais
echo "📦 Instalando ferramentas principais..."
pkg install x11-repo -y &>/dev/null
pkg install pulseaudio -y &>/dev/null
pkg install xwayland -y &>/dev/null
pkg install wget -y &>/dev/null
pkg install tsu -y &>/dev/null
pkg install root-repo -y &>/dev/null
pkg install patchelf -y &>/dev/null
pkg install p7zip -y &>/dev/null
pkg install xorg-xrandr -y &>/dev/null
pkg install ncurses-utils -y &>/dev/null
pkg install hashdeep -y &>/dev/null
pkg install termux-x11-nightly -y &>/dev/null

# Limpa versão anterior se existir
if [ -e $PREFIX/glibc ]; then
  echo ""
  echo "🗑️  Removendo versão anterior..."
  rm -rf $PREFIX/glibc
fi

echo ""
echo "⚙️  CONFIGURAÇÃO DO ATITUDE BOX"
echo "----------------------------------------"
echo "Escolha a versão para instalar:"
echo "1) Versão Estável"
echo "2) Versão Nova (WOW64 - Recomendada)"
echo ""
read -p "Digite o número da opção: " i

INSTALL_WOW64=0
if [ "$i" = "2" ]; then
  INSTALL_WOW64=1
fi

echo ""
echo "⬇️  Baixando núcleo do Atitude Box..."
echo "⚠️  ISSO VAI DEMORAR! IGUAL AO ORIGINAL! ⚠️"
echo "   Não feche o app, deixe trabalhando..."
echo ""

# Função de download
wget-git-q() {
  wget -q --retry-connrefused --tries=0 "https://gitlab.com/api/v4/projects/$PROJECT_ID/repository/files/$1/raw?ref=main" -O $2
}

# Define IDs dos projetos
if [ "$INSTALL_WOW64" = "1" ]; then
  PROJECT_ID=54240888
else
  PROJECT_ID=52465323
fi

mkdir -p $PREFIX/glibc/opt/package-manager
echo "$PROJECT_ID" > $PREFIX/glibc/opt/package-manager/token

# Baixa e executa o gerenciador
wget-git-q "package-manager" "$PREFIX/glibc/opt/package-manager/package-manager"
chmod +x "$PREFIX/glibc/opt/package-manager/package-manager"
. $PREFIX/glibc/opt/package-manager/package-manager

sync-all

if [ "$INSTALL_WOW64" = "1" ]; then
  sync-package wine-9.3-vanilla-wow64
else
  sync-package wine-ge-custom-8-25
fi

# CRIA O COMANDO PRINCIPAL CERTINHO
echo "🔗 Criando atalho..."
mkdir -p $PREFIX/bin
cp -f $PREFIX/glibc/opt/scripts/mobox $PREFIX/bin/atitudebox
chmod +x $PREFIX/bin/atitudebox

echo ""
echo "✅ =============================================="
echo "✅  INSTALAÇÃO CONCLUÍDA COM SUCESSO! 🇧🇷      "
echo "✅                                             "
echo "✅  Para iniciar o emulador, digite:           "
echo "✅             atitudebox                      "
echo "✅                                             "
echo "✅  Não é só emular, é ter ATITUDE! 🚀💪      "
echo "✅ ============================================="https://gitlab.com/api/v4/projects/$PROJECT_ID/repository/files/$1/raw?ref=main
