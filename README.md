# 🧾 NFe DANFE PDF

> **Biblioteca robusta e completa para geração de DANFE (NF-e e NFC-e) em PDF para aplicações Node.js**

[![NPM Version](https://img.shields.io/npm/v/nfe-danfe-pdf.svg)](https://www.npmjs.com/package/nfe-danfe-pdf)
[![TypeScript](https://img.shields.io/badge/TypeScript-Ready-blue.svg)](https://www.typescriptlang.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 🚀 **Funcionalidades Principais**

### ✅ **Suporte Completo**

- **NF-e (Modelo 55)**: Documentos fiscais eletrônicos com layout profissional
- **NFC-e (Modelo 65)**: Cupom fiscal eletrônico otimizado
- **Conformidade 100%**: Atende às especificações técnicas da Receita Federal (MOC)
- **TypeScript Nativo**: Tipagem completa e IntelliSense

### 🎨 **Recursos Avançados**

- **Layout Responsivo**: Adapta-se automaticamente ao conteúdo
- **Quebra Inteligente de Texto**: Processa informações complementares longas
- **Logos Personalizadas**: Suporte a imagens em diferentes formatos
- **Código de Barras**: Geração automática conforme padrões oficiais
- **QR Code**: Para NFC-e com validação automática
- **Múltiplas Páginas**: Suporte a documentos com muitos itens

### 🛡️ **Qualidade e Conformidade**

- **Validação XML**: Parser robusto com tratamento de erros
- **Campos Obrigatórios**: Verificação automática de dados essenciais
- **Formatação Automática**: CNPJ, CPF, CEP, datas e valores monetários
- **Notas Canceladas**: Marcação visual clara para documentos cancelados
- **Homologação**: Marca d'água automática para ambiente de testes

---

## 📦 **Instalação**

```bash
# NPM
npm install nfe-danfe-pdf

# Yarn
yarn add nfe-danfe-pdf

# PNPM
pnpm add nfe-danfe-pdf
```

---

## 🎯 **Uso Básico**

### **Exemplo Simples**

```typescript
import { gerarPDF } from 'nfe-danfe-pdf';
import fs from 'fs';

async function main() {
  try {
    // Carregando XML da NFe
    const xmlContent = fs.readFileSync('./nota-fiscal.xml', 'utf8');

    // Gerando PDF
    const pdfDoc = await gerarPDF(xmlContent);

    // Salvando arquivo
    const writeStream = fs.createWriteStream('./danfe.pdf');
    pdfDoc.pipe(writeStream);

    // Aguardar finalização do arquivo
    await new Promise<void>((resolve, reject) => {
      writeStream.on('finish', () => {
        console.log('PDF gerado com sucesso!');
        resolve();
      });

      writeStream.on('error', reject);
      pdfDoc.on('error', reject);
    });
  } catch (error) {
    console.error('Erro:', error);
  }
}

main();
```

### **Exemplo Avançado**

```typescript
import { gerarPDF } from 'nfe-danfe-pdf';
import fs from 'fs';

const xmlContent = fs.readFileSync('./nfe.xml', 'utf8');

// Opções personalizadas
const opcoes = {
  pathLogo: './assets/logo-empresa.png', // Logo da empresa
  cancelada: false, // Nota não cancelada
  textoRodape: 'Meu Sistema Danfe' // Texto personalizado no rodapé
};

const pdfDoc = await gerarPDF(xmlContent, opcoes);

// Salvando com callback
pdfDoc.pipe(fs.createWriteStream('./danfe-personalizado.pdf'));
pdfDoc.on('end', () => {
  console.log('✅ DANFE gerado com sucesso!');
});
pdfDoc.end();
```

---

## ⚙️ **API Completa**

### **`gerarPDF(xml, opcoes?)`**

**Parâmetros:**

- **`xml`** `string`: Conteúdo XML da NF-e/NFC-e
- **`opcoes`** `OpcoesPDF` _(opcional)_: Configurações personalizadas

**Retorna:** `Promise<PDFKit.PDFDocument>`

### **Tipo `OpcoesPDF`**

```typescript
type OpcoesPDF = {
  pathLogo?: string; // Caminho para logo da empresa
  cancelada?: boolean; // Marcar nota como cancelada
  textoRodape?: string; // Texto personalizado no rodapé do documento
};
```

#### **Detalhes dos Parâmetros:**

- **`pathLogo`**: Caminho para arquivo de imagem da logo da empresa (PNG, JPG, etc.)
- **`cancelada`**: Define se a nota deve ser marcada visualmente como cancelada
- **`textoRodape`**: Texto personalizado que aparece no rodapé direito do documento (ex: "Meu Sistema Danfe")

#### **Exemplo com Rodapé Personalizado:**

```typescript
import { gerarPDF } from 'nfe-danfe-pdf';
import fs from 'fs';

const xmlContent = fs.readFileSync('./nfe.xml', 'utf8');

// Opções com texto personalizado no rodapé
const opcoes = {
  pathLogo: './assets/logo-empresa.png',
  cancelada: false,
  textoRodape: 'Meu Sistema Danfe' // Texto que aparece no canto inferior direito
};

const pdfDoc = await gerarPDF(xmlContent, opcoes);
pdfDoc.pipe(fs.createWriteStream('./danfe-com-rodape.pdf'));
```

**💡 Resultado Visual do Rodapé:**

```
Impresso em 27/09/2025 às 15:30:45                    Meu Sistema Danfe
    ↑ (lado esquerdo)                                      ↑ (lado direito)
```

---

## 🎨 **Recursos Visuais**

### **Layout Profissional**

- ✅ Cabeçalho com dados do emitente e logo
- ✅ Seções organizadas e bem delimitadas
- ✅ Tabela de itens com colunas alinhadas
- ✅ Informações fiscais destacadas
- ✅ Dados adicionais com altura dinâmica
- ✅ **Rodapé personalizado** com data/hora e texto customizável

### **Formatações Automáticas**

- 📄 **CNPJ/CPF**: `12.345.678/0001-90` | `123.456.789-00`
- 📅 **Datas**: `26/09/2025 15:30:45`
- 💰 **Valores**: `R$ 1.234,56`
- 📮 **CEP**: `12345-678`
- 🔑 **Chave NFe**: `1234 5678 9012 3456 7890 1234 5678 9012 3456 7890 1234`

### **Tratamento Inteligente**

- 📝 **Textos Longos**: Quebra automática em múltiplas linhas
- 🔄 **Altura Dinâmica**: Seções expandem conforme o conteúdo
- 🎯 **Alinhamentos**: Centro, esquerda, direita, justificado
- 🎨 **Fontes**: Times New Roman e Barlow Condensed incluídas

---

## 📋 **Campos Suportados**

### **Identificação**

- ✅ Dados do emitente (razão social, CNPJ, endereço)
- ✅ Destinatário/remetente completo
- ✅ Dados da nota (número, série, data, chave)
- ✅ Protocolo de autorização

### **Produtos/Serviços**

- ✅ Tabela completa de itens
- ✅ Código, descrição, quantidade, valor unitário
- ✅ Valores de ICMS, IPI, PIS, COFINS
- ✅ Classificação fiscal (NCM, CEST)

### **Valores e Impostos**

- ✅ Totais da nota (produtos, impostos, desconto)
- ✅ Tributos aproximados (Lei da Transparência)
- ✅ Informações de ICMS, ISS, substituição tributária
- ✅ Modalidades de pagamento

### **Informações Adicionais**

- ✅ Fatura e duplicatas (com vencimentos)
- ✅ Dados de transporte (transportadora, volumes)
- ✅ Informações complementares (campo livre)
- ✅ Informações do fisco

---

## 🔧 **Integração com Frameworks**

### **Express.js**

```typescript
import express from 'express';
import { gerarPDF } from 'nfe-danfe-pdf';

const app = express();

app.post('/gerar-danfe', async (req, res) => {
  try {
    const { xml, pathLogo, textoRodape } = req.body;
    const pdfDoc = await gerarPDF(xml, { pathLogo, textoRodape });

    res.setHeader('Content-Type', 'application/pdf');
    res.setHeader('Content-Disposition', 'attachment; filename="danfe.pdf"');

    pdfDoc.pipe(res);
    pdfDoc.end();
  } catch (error) {
    res.status(500).json({ erro: 'Erro ao gerar DANFE' });
  }
});
```

### **NestJS**

```typescript
import { Controller, Post, Body, Res, HttpStatus } from '@nestjs/common';
import { Response } from 'express';
import { gerarPDF } from 'nfe-danfe-pdf';

@Controller('danfe')
export class DanfeController {
  @Post('gerar')
  async gerarDanfe(
    @Body('xml') xml: string,
    @Body('pathLogo') pathLogo?: string,
    @Body('textoRodape') textoRodape?: string,
    @Res() res: Response
  ) {
    try {
      const pdfDoc = await gerarPDF(xml, { pathLogo, textoRodape });

      res.setHeader('Content-Type', 'application/pdf');
      pdfDoc.pipe(res);
      pdfDoc.end();
    } catch (error) {
      res.status(HttpStatus.INTERNAL_SERVER_ERROR).json({ message: 'Erro ao gerar DANFE' });
    }
  }
}
```

---

## 🌐 **Implementação em Projetos Front-end**

### **React + TypeScript**

Para usar em projetos React, você precisa de um endpoint backend que gere o PDF:

```tsx
import React, { useState } from 'react';
import axios from 'axios';

interface DanfeGeneratorProps {
  xmlContent: string;
}

const DanfeGenerator: React.FC<DanfeGeneratorProps> = ({ xmlContent }) => {
  const [isGenerating, setIsGenerating] = useState(false);

  const gerarDanfe = async () => {
    setIsGenerating(true);
    try {
      const response = await axios.post(
        '/api/gerar-danfe',
        {
          xml: xmlContent,
          pathLogo: '/assets/logo.png',
          textoRodape: 'Meu Sistema Danfe'
        },
        {
          responseType: 'blob' // Importante para receber PDF
        }
      );

      // Criar URL do blob e fazer download
      const blob = new Blob([response.data], { type: 'application/pdf' });
      const url = window.URL.createObjectURL(blob);

      // Criar link de download
      const link = document.createElement('a');
      link.href = url;
      link.download = 'danfe.pdf';
      document.body.appendChild(link);
      link.click();

      // Cleanup
      document.body.removeChild(link);
      window.URL.revokeObjectURL(url);
    } catch (error) {
      console.error('Erro ao gerar DANFE:', error);
      alert('Erro ao gerar DANFE');
    } finally {
      setIsGenerating(false);
    }
  };

  return (
    <button
      onClick={gerarDanfe}
      disabled={isGenerating}
      className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 disabled:opacity-50"
    >
      {isGenerating ? 'Gerando...' : 'Gerar DANFE'}
    </button>
  );
};

export default DanfeGenerator;
```

### **Vue.js + TypeScript**

```vue
<template>
  <div>
    <button
      @click="gerarDanfe"
      :disabled="isGenerating"
      class="px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600 disabled:opacity-50"
    >
      {{ isGenerating ? 'Gerando...' : 'Gerar DANFE' }}
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

interface Props {
  xmlContent: string;
  logoPath?: string;
}

const props = defineProps<Props>();
const isGenerating = ref(false);

const gerarDanfe = async () => {
  isGenerating.value = true;

  try {
    const response = await axios.post(
      '/api/gerar-danfe',
      {
        xml: props.xmlContent,
        pathLogo: props.logoPath
      },
      {
        responseType: 'blob'
      }
    );

    // Download do PDF
    const blob = new Blob([response.data], { type: 'application/pdf' });
    const url = URL.createObjectURL(blob);

    const link = document.createElement('a');
    link.href = url;
    link.download = 'danfe.pdf';
    link.click();

    URL.revokeObjectURL(url);
  } catch (error) {
    console.error('Erro ao gerar DANFE:', error);
  } finally {
    isGenerating.value = false;
  }
};
</script>
```

### **Angular + TypeScript**

**Serviço (danfe.service.ts):**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class DanfeService {
  constructor(private http: HttpClient) {}

  gerarDanfe(xmlContent: string, logoPath?: string): Observable<Blob> {
    const headers = new HttpHeaders({
      'Content-Type': 'application/json'
    });

    return this.http.post(
      '/api/gerar-danfe',
      {
        xml: xmlContent,
        pathLogo: logoPath
      },
      {
        headers,
        responseType: 'blob'
      }
    );
  }

  downloadPdf(blob: Blob, filename: string = 'danfe.pdf'): void {
    const url = window.URL.createObjectURL(blob);
    const link = document.createElement('a');
    link.href = url;
    link.download = filename;
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    window.URL.revokeObjectURL(url);
  }
}
```

**Componente (danfe.component.ts):**

```typescript
import { Component } from '@angular/core';
import { DanfeService } from './danfe.service';

@Component({
  selector: 'app-danfe',
  template: `
    <button
      (click)="gerarDanfe()"
      [disabled]="isGenerating"
      class="px-4 py-2 bg-purple-500 text-white rounded hover:bg-purple-600 disabled:opacity-50"
    >
      {{ isGenerating ? 'Gerando...' : 'Gerar DANFE' }}
    </button>
  `
})
export class DanfeComponent {
  isGenerating = false;

  constructor(private danfeService: DanfeService) {}

  gerarDanfe(): void {
    this.isGenerating = true;

    this.danfeService.gerarDanfe(this.xmlContent).subscribe({
      next: (blob) => {
        this.danfeService.downloadPdf(blob);
        this.isGenerating = false;
      },
      error: (error) => {
        console.error('Erro ao gerar DANFE:', error);
        this.isGenerating = false;
      }
    });
  }

  // Propriedade que deve ser definida com o XML
  xmlContent = ''; // Seu XML aqui
}
```

### **Next.js (App Router)**

**API Route (app/api/gerar-danfe/route.ts):**

```typescript
import { gerarPDF } from 'nfe-danfe-pdf';
import { NextRequest, NextResponse } from 'next/server';

export async function POST(request: NextRequest) {
  try {
    const { xml, pathLogo } = await request.json();

    const pdfDoc = await gerarPDF(xml, { pathLogo });

    // Converter PDFKit document para Buffer
    const chunks: Buffer[] = [];

    return new Promise((resolve) => {
      pdfDoc.on('data', (chunk) => chunks.push(chunk));
      pdfDoc.on('end', () => {
        const pdfBuffer = Buffer.concat(chunks);

        resolve(
          new NextResponse(pdfBuffer, {
            status: 200,
            headers: {
              'Content-Type': 'application/pdf',
              'Content-Disposition': 'attachment; filename="danfe.pdf"'
            }
          })
        );
      });
      pdfDoc.end();
    });
  } catch (error) {
    return NextResponse.json({ error: 'Erro ao gerar DANFE' }, { status: 500 });
  }
}
```

**Cliente (components/DanfeButton.tsx):**

```tsx
'use client';

import { useState } from 'react';

interface DanfeButtonProps {
  xmlContent: string;
  logoPath?: string;
}

export default function DanfeButton({ xmlContent, logoPath }: DanfeButtonProps) {
  const [isGenerating, setIsGenerating] = useState(false);

  const handleDownload = async () => {
    setIsGenerating(true);

    try {
      const response = await fetch('/api/gerar-danfe', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          xml: xmlContent,
          pathLogo: logoPath,
          textoRodape: 'Meu Sistema Danfe'
        })
      });
      if (!response.ok) {
        throw new Error('Erro ao gerar DANFE');
      }

      const blob = await response.blob();
      const url = URL.createObjectURL(blob);

      const link = document.createElement('a');
      link.href = url;
      link.download = 'danfe.pdf';
      link.click();

      URL.revokeObjectURL(url);
    } catch (error) {
      console.error('Erro:', error);
      alert('Erro ao gerar DANFE');
    } finally {
      setIsGenerating(false);
    }
  };

  return (
    <button
      onClick={handleDownload}
      disabled={isGenerating}
      className="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700 disabled:opacity-50"
    >
      {isGenerating ? 'Gerando DANFE...' : 'Download DANFE'}
    </button>
  );
}
```

### **🔑 Pontos Importantes para Front-end**

#### **Segurança**

- ⚠️ **Nunca processe XMLs no cliente**: Sempre use um backend
- 🔐 **Validação**: Valide XMLs no servidor antes do processamento
- 🛡️ **Autenticação**: Proteja endpoints de geração de PDF

#### **UX/UI**

- ⏳ **Loading States**: Sempre mostre feedback durante geração
- 📱 **Responsivo**: Considere diferentes dispositivos
- 💾 **Download Automático**: Use `Content-Disposition: attachment`
- ❌ **Tratamento de Erros**: Exiba mensagens claras ao usuário

#### **Performance**

- 🚀 **Cache**: Considere cache de PDFs gerados recentemente
- 📊 **Limite**: Implemente rate limiting nos endpoints
- 🔄 **Timeout**: Configure timeouts adequados para requisições

---

## 🧪 **Requisitos e Compatibilidade**

### **Node.js**

- ✅ **Versão Mínima**: Node.js 20.19+ ou 22.12+
- ✅ **Recomendado**: Node.js 22 LTS ou superior
- ✅ **TypeScript**: 7.0+ no desenvolvimento da biblioteca

### **Formatos XML Suportados**

- ✅ **NFe Processada**: `<nfeProc>` (com protocolo)
- ✅ **NFCe Processada**: `<nfceProc>` (com protocolo)
- ✅ **Encoding**: UTF-8, ISO-8859-1
- ✅ **Versões**: Layout 4.00 (atual)

### **Dependências Principais**

- **PDFKit**: Geração de PDF de alta qualidade
- **xml2js**: Parser XML robusto
- **date-fns**: Manipulação de datas
- **qrcode**: Geração de QR codes
- **bwip-js**: Códigos de barras

---

## 📊 **Performance**

- ⚡ **Velocidade**: ~500ms para NFe média (20 itens)
- 💾 **Memória**: ~10MB para documentos complexos
- 📄 **Tamanho**: PDFs otimizados (~50KB-200KB)
- 🔄 **Concorrência**: Suporte a múltiplas instâncias simultâneas

---

## 🛠️ **Desenvolvimento**

### **Scripts Disponíveis**

```bash
npm run build     # Compilar TypeScript
npm run test      # Executar testes
npm run lint      # Analisar o código com Oxlint
npm run prettier  # Formatar código
npm run type      # Verificar tipos
```

### **Estrutura do Projeto**

```
src/
├── domain/          # Lógica de negócio
├── application/     # Casos de uso
├── types/          # Definições TypeScript
└── utils/          # Utilitários
```

---

## 📄 **Licença**

Este projeto está licenciado sob a **MIT License** - veja o arquivo [LICENSE](LICENSE) para detalhes.

---

## 🤝 **Contribuição**

Contribuições são bem-vindas! Por favor:

1. Faça um fork do projeto
2. Crie sua feature branch (`git checkout -b feature/nova-funcionalidade`)
3. Commit suas mudanças (`git commit -am 'Adiciona nova funcionalidade'`)
4. Push para a branch (`git push origin feature/nova-funcionalidade`)
5. Abra um Pull Request

---

## 📞 **Suporte**

- 🐛 **Issues**: [GitHub Issues](https://github.com/flaviosoliver/nfe-danfe-pdf/issues)
- 📧 **Email**: [flavsoliver@gmail.com](mailto:flavsoliver@gmail.com)

---

<div align="center">

**⭐ Se este projeto te ajudou, considere dar uma estrela no repositório! ⭐**

</div>
