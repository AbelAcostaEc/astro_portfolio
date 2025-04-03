---
    title: 'Contenido Pagina'
    layout: '../../layouts/Layout.astro'
---
# Ionic

1.- Comandos principales
    ionic start my-app 
    -- permite crear una aplicacion
    ionic serve
    -- permite correr apliacion
    ionic g m my-module  --routing
    -- permite crear un modulo --routing si va a manejar rutas
    ionic g c my-component --spec=false
    -- permite crear componente --spec omite archivo test
    ionic g i models/model
    -- permite crear una interface
    ionic g s services/test
    -- permite crear un servicio
    ionic g p shared/pipes/miPipe
    -- permite crear un pipe
    ionic g d shared/directives/miDirective
    -- pertmite crear una directiva
    ionic g g sahred/guards/myGuard

2.- Estrucutra Proyecto

    |src
        |app
            |module
                |components
                |pages
            |models (interfaces)
            |services
            |shared
                |component
                |pages
                    |not-found


3.- Creacion Modulo, componente y page
    1.- genera modulo
        importar IonContent 
            import { IonContent } from '@ionic/angular/standalone';
    2.- generar page (componente)
        Pagina standalone false
        Declaration en el modulo
        registrar ruta apuntando al componente en routing module
            {path: '', component: HomeComponent},
        Registrar Ruta en app routes (importa el modulo para heredar rutas loadChildren) 
            {
                path: 'home',
                loadChildren: () => import('./home/home.module').then(module => module.HomeModule),
            },
        Vista Utilizar ion-content contenedor principal
        <ion-content>
        </ion-content>
    3.- generar componente (usarlo en el modulo)
        Componente standalone false
        Declaration en el modulo

4.- Shared Module (componentes compartidos toda la app)
    1.- genera modulo sin routing
    2.- generar componente
        Componente standalone false
        Declaration y export en el modulo
    3.- Uso componente
        En el Modulo que se requiere se Importa el ModuloShared
    4.- Page NotFound (componente standalone) 
        - Nota: standalone funciona como que el compentne es un mdoulo
        generar componente
        en @Component imports el IonContent y RouterModule
        Registrar en app routes ruta para cuando no exista
            {
                path: '**', // cualquier ruta que no exista
                loadComponent: () => import('./shared/pages/not-found/not-found.component').then(module => module.NotFoundComponent)
            }
        Vista 
        redireccionar con routerLink (uso de RouterModule)
            <ion-content>
            <button class="button-back" [routerLink]="['/']">back</button>
            </ion-content>

5.- Navegacion entre componentes (pages)
    1.- en el moudlo (si es que no es routing) o si es standalone importar RouterModule
    2.- utilizar  [routerLink]="['/store/home']"
    3.- utilizar [routerLink]="['/home/article/' + article.id]"

6.- Permite declarar variables sin inicializar antes
    en tsconfig.jon en compilerOptions añadir
    "strictPropertyInitialization": false,
    "strictNullChecks": false,

7.- Interfaces
    1.- crear interface requerida models/home.models.ts
        -- interface agrupar con namespace
        export namespace ModelsHome {
            export interface Article {
                id?: string;
                title: string;
                description: string;
                image: {
                    url: string;
                    alt: string;
                }
            }
        }
    2.- crear interface principal models/models.ts
        -- interface que se va a importar en donde se requiera
            import { ModelsHome } from 'src/app/models/home.models';

            export namespace Models {
                export import Home = ModelsHome;
            }
    3.- Usar en componente
        - importar  import { Models } from 'src/app/models/models';
        - definir variable article: Models.Home.Article;
        - definir valor segun corresponda
        - en vista utilizar
            @if(article){
                <h2>{{ article.title }}</h2>
                <p>{{ article.description }}</p>
            }

8.- uso de eventos
    - en una vista usar 
        -- si se quiere capturar evento pasar $event y en funcion recibir event:any
        <button (click)="viewItem(item)">Ver</button>
    - en el componente.ts crear la funcion definida
        viewItem(item: Models.Home.Article) {
            console.log('View Item', item);
        }

9.- Uso de Input (variables entre componentes)
    - en el componente importar 
    import { Component, Input, OnInit } from '@angular/core';
    - definir la variable
    @Input() title: string;
    - en el componente pasar la variable
    <app-header title="hola"></app-header>
    -- si se desea enviar una variable utilizar corchetes
    <app-header [title]="title" ></app-header>

10.- Uso de Output
    - en el componente importar
        import { Component, EventEmitter, OnInit, Output } from '@angular/core';
    - definir la variable
        @Output() clickEvent = new EventEmitter();
    - definir una accion que ejecute el output
        this.clickEvent.emit(item);
    - en donde se desea usar
        <app-article (clickEvent)="customFunction()"></app-article>

11.- Uso de Servicios
    - crear el servicio en services/myservice
    - definir el servicio en el componente
        private testService = inject(TestService);
    - utilizar this.testService.testFunction();
    
12.- Uso de clases y estilos dinamicos
    - en el elemnto html añadir  
    - clase
        [ngClass]="{'empty-cart': qtyCart == 0}"
    - estilo
        [ngStyle]="{ background: color, 'border-radius': '10px' }"

13.- Uso de condicionales y bucles
    - IF
        @if(article){
            <div class="">
                <h2>{{ article.title }}</h2>
            </div>
        }
    - IF Antiguo
    <div *ngIf="article; else articleNotFound">
        <div class="">
            <h2>{{ article.title }}</h2>
        </div>
    </div>
    <ng-template #articleNotFound>
        <div>
            NO HAY ARTICULOS
        </div>
    </ng-template>
    -FOR
     @for (article of articles; track $index) {
        <app-article [article]="article"></app-article>
    } @empty {
        <p>no hay articulos</p>
    }
    -FOR ANTIGUO
    <div *ngFor="let article of articles; index as i">
      <app-article [article]="article"></app-article>
    </div>

14.- rutas con parametros
    1.- generar componente page
    2.- definir la ruta en el routing del module
        {path: 'article/:id', component:ArticlePageComponent}
    3.- Redirigir a la ruta
        1.- routerLink
        [routerLink]="['/home/article/' + article.id]"
        2.- Router
            - En constructor de componente definir
                import { Router } from '@angular/router';
                constructor(private router: Router){}
            - utilizar el click a una funcion especifica
                (click)="goToArticle()"
            - funcion redirigir usando el router
                goToArticle(){
                    this.router.navigate(['/home/article', this.article.id]);
                }
    4.- Obtener variable en page
        1.- definir el activeRoute
            private route = inject(ActivatedRoute);

        2.- En constructor obtener los params
            this.route.params.subscribe(params => {
                console.log(params);
                if(params.id){}
            })

15.- rutas query params
    1.- cualquier pagina para enviar utilizar queryparams
    <button [routerLink]="['/home/article]"
      [queryParams]="{title:'dsadas', other:'other}">Ver</button>
    2.- Router
        - En constructor de componente definir
            import { Router } from '@angular/router';
            constructor(private router: Router){}
        - utilizar el click a una funcion especifica
            (click)="goToArticle()"
        - funcion redirigir usando el router
            goToArticle(){
                this.router.navigate(['/home/article', this.article.id], { queryParams: { title: this.article.title } });
            }
    3.- Obtener variable en page
        1.- definir el activeRoute
            private route = inject(ActivatedRoute);

        2.- En constructor obtener los params
            this.route.queryParams.subscribe(queryParams => {
            console.log(queryParams);
            })

16.- Uso NgModel (parecido al wire:model de livewire)
    1.- en el modulo importar FormsModule    
        import { FormsModule } from '@angular/forms';
        imports: [
            FormsModule
        ]
    2.- En el componente definir la variable o interfaz
        form:Models.Contact.FormContactI ={
            email: '',
            name: '',
            phone: '',
        }
    3.- En los input definir el ngModel con la propiedad de la interfaz
        <input type="mail" [(ngModel)]="form.email" />
    4.- Al enviar del form se puede validar
        sendForm(){
            if(!this.form.name){
            this.error = 'Name is required';
            return;
            }
            console.log('sendForm');
            console.log("Form: ", this.form);
        }

17.- Formularios reactivos
    1.- en el modulo importar ReactiveFormsModule    
        import { ReactiveFormsModule } from '@angular/forms';
        imports: [
            ReactiveFormsModule
        ]
    2.- En el componente inyectar el FormBuilder
        constructor(private fb: FormBuilder) { }
    3.- Definir variable datos del formulairo con sus reglas de validacion
        formData = this.fb.group({
            email: ['', Validators.required],
            name: ['', Validators.required],
            phone: [''],
        })
    4.- crear un formulario con un evento submit
        <form [formGroup]="formData" (ngSubmit)="sendFormReactive()">
            <button type="submit" [disabled]="!formData.valid">
                <strong>Enviar</strong>
            </button>
        </form>
    5.- Crear un input con la formControlName definida
        <input placeholder="Email" formControlName="email" />
    6.- Mostrar errores de validador
        <span *ngIf="formData.controls['email'].hasError('required')">
            <i>Este campo es <strong>requerido</strong></i>
        </span>
    7.- Validador personalizado 
        1.- definir una funcion que reciba el input tipo FormControl y  retornar la calve que queremos validar
        customValidator(input: FormControl){
            console.log('input: ', input.value);
            if(input.value.length != 10){
                return {customLength: true}
            }
            return {};
        }
        2.- Añadir la regla de validacion
            phone: ['', [Validators.required, this.customValidator]],
        3.- mostrar el error con la variable definida
            <span *ngIf="formData.controls['phone'].hasError('customLength')">
                <i>Mensaje de validacion custom</i>
            </span>
    8.- Estar pendiente a cambio de un input, en el ngOnInit suscribir al cambio
        ngOnInit() { 
            this.formData.controls['email'].valueChanges.subscribe((value) => {
                console.log('Email value: ', value);
            })
        }
    9.- Asignar el valor a un input, en una funcion
        this.formData.controls['phone'].setValue('1234567890');

18.- Pipes
    - utiliza en la vista para formatear valores
        <p>{{ article.time |date:'yyyy' }}</p>
        <h2>{{ article.title |uppercase }}</h2>
        <p>{{ item.price |currency }}</p>
        {{ article |json }}
        <p>{{ article.body | uppercase | currency }}</p>

    - pipe personalizado
        1.- crear el pipe y definir standalone: true
        2.- Importar en el modulo que se requiera usar
            imports: [ResumePipe]
        3.- personalizar pipe
            /**
            * 
            * @param value Deinir el tipo de valor a transformar
            * @param args 
            * @returns 
            */
            transform(value: string, ...args: unknown[]): unknown {
                console.log('args -> ', args);
                console.log('value -> ', value);
                let maxLength = args[0] as number;
                if(value.length > maxLength) {
                return value.slice(0, maxLength) + '...';
                }
                
                return value;
            }
        4.- utilizar el pipe y pasar parametros
            <p>{{ article.body | resume:'30':'param2' }}</p>

19.- Directivas
    1.- Crear la directiva como standalone
    2.- Importar en el modulo que se requiera usar
        imports: [HighlightDirective]
    3.- Definir en el constuctor que realizar
        constructor(private el: ElementRef) {
            this.el.nativeElement.style.backgroundColor = 'yellow';
        }
    4.- Definir un evento para ejecutar la accion
        @HostListener('mouseleave') onMouseLeave() {
            this.highlight('');
        }
        private highlight(color: string) {
            this.el.nativeElement.style.backgroundColor = color;
        }
    5.- definir inputs 
        @Input() appHighlight = '';
        @Input() nameCustom = 'default';
    6.- Pasar inputs desde html
        <h2 appHighlight="red" nameCustom="pepito"></h2>
    7.- utilizar input y evento personalizado
        @HostListener('click') registerEvent() {
            console.log('click event');
            console.log('name -> ', this.nameCustom);
        }

20.- Guards
    1.- Crear el guard -> CanActivate
    2.- En el routing del module requerido  importar el canactivate
        path: '', component: NotificationsComponent, canActivate: [isAdminGuard]
    3.- Utilizar funciones personalizadas
        - Inyectar providedIn
            @Injectable({
                providedIn: 'root'
            })
        - definir variables
            export class isAdminGuard {
                async canActivate(
                    route: ActivatedRouteSnapshot,
                    state: RouterStateSnapshot
                ): Promise<boolean> {
                    const is = await this.isAdmin();
                    return is;
                }

                isAdmin() {
                    return new Promise<boolean>((resolve, reject) => {
                        setTimeout(() => {
                            resolve(true);
                        }, 1000);
                    });
                }
            }
    4.- Forma nueva
        export const isAdminGuard: CanActivateFn = async (
            next: ActivatedRouteSnapshot,
            state: RouterStateSnapshot) => {

            const webService = inject(WebService);
            // await true;
            return true;
        }


# Capacitor
1.- ejecutar ionic build --prod
## Android
1.- npm install @capacitor/android
2.- capacitor.config.ts modificar id y name
    const config: CapacitorConfig = {
    appId: 'capacitor.demo.app',
    appName: 'Capacitor-demo',
    webDir: 'www'
    };
3.- npx cap add android
4.- npx cap open android
5.- npx cap sync android



# IONIC Components
    -Importar el Componente 
    -- si es estandalon en imports, sino en su modulo correspondiente.
