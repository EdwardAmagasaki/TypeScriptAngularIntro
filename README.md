
# Introdução ao TypeScript e Angular

Angular é um framework de desenvolvimento web baseado em TypeScript, criado pelo Google. Ele facilita a criação de aplicações web robustas e escaláveis. Neste post, vamos criar uma aplicação simples em Angular que exibe uma lista de itens categorizados.

## Estrutura do Projeto

### Entidade Categoria

**TypeScript**

```typescript
export class Categoria {
    id: number;
    nome: string;
}
```

### Entidade Item

**TypeScript**

```typescript
export class Item {
    id: number;
    nome: string;
    categoriaId: number;
    categoria: Categoria;
}
```

### Serviço para Gerenciar Dados

**TypeScript**

```typescript
import { Injectable } from '@angular/core';
import { Categoria } from './categoria';
import { Item } from './item';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  categorias: Categoria[] = [
    { id: 1, nome: 'Eletrônicos' },
    { id: 2, nome: 'Moda' }
  ];

  itens: Item[] = [
    { id: 1, nome: 'Smartphone XYZ', categoriaId: 1, categoria: this.categorias[0] },
    { id: 2, nome: 'Fone de Ouvido ABC', categoriaId: 1, categoria: this.categorias[0] },
    { id: 3, nome: 'Camisa Estilosa', categoriaId: 2, categoria: this.categorias[1] },
    { id: 4, nome: 'Tênis Confortável', categoriaId: 2, categoria: this.categorias[1] }
  ];

  getItens() {
    return this.itens;
  }
}
```

### Componente para Exibir Itens

**TypeScript**

```typescript
import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';
import { Item } from './item';

@Component({
  selector: 'app-item-list',
  template: `
    <table>
      <thead>
        <tr>
          <th>Nome</th>
          <th>Categoria</th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let item of itens">
          <td>{{ item.nome }}</td>
          <td>{{ item.categoria.nome }}</td>
        </tr>
      </tbody>
    </table>
  `,
  styles: [`
    table {
      width: 100%;
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid #ddd;
      padding: 8px;
    }
    th {
      background-color: #f2f2f2;
    }
  `]
})
export class ItemListComponent implements OnInit {
  itens: Item[];

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.itens = this.dataService.getItens();
  }
}
```

### Módulo Principal

**TypeScript**

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { ItemListComponent } from './item-list.component';
import { DataService } from './data.service';

@NgModule({
  declarations: [
    AppComponent,
    ItemListComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [DataService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
