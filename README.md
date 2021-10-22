# Desafio TGT
 Desafio TGT Nest.js, Angular e Mongodb
 Todas as APIs devidamente testadas.
 

 Banco de dados MongoDB com Cluster criado, os ips ja foram liberados para reliazacao de teste.
 link Mongo Compass- mongodb+srv://john:321@clusterdesafio.z6hwr.mongodb.net/test
 
 -------------------------------------------------------//----------------------------------------------------
 Back-end desenvolvido em Nest.js
 
 CRUD- Cadastro de usuario utilizando API.
 
 
 CORS configurado para todos os dominios para realizacao de teste.
 app.enableCors({
    origin: '*',
    methods: 'GET, PUT, POST, DELETE, PATCH',
    allowedHeaders: 'Content-Type, Authorization',
});
 
 Comando nest g resource para criacao e estruturacao do CRUD para USERS.
 
 users.service configurado de acordo com o Mongo
 Conexao com o banco devidamente configurada utilizando Mongoose.
 app.module devidamente configurado realizando a conexao com o Mongo MongooseModule.forRoot('mongodb+srv://john:321@clusterdesafio.z6hwr.mongodb.net/test'),
 
 schema do banco devidamente criado nas entities.
 
 E requisicoes com o mongo devidamente configuradas em users.service:
   create(createUserDto: CreateUserDto) {
    const user = new this.userModel(createUserDto);
    return user.save();
  }

  findAll() {
    return this.userModel.find();
  }

  findOne(id: string) {
    return this.userModel.findById(id);
  }

  update(id: string, updateUserDto: UpdateUserDto) {
    return this.userModel.findByIdAndUpdate({
      _id: id
    },{
      $set: updateUserDto,
    },{
      new: true,
    }
    );
  }

  remove(id: string) {
    return this.userModel.deleteOne({
      _id: id,
    }).exec();
  }
 
 create.user.dto devidamente configurado:
    name: string;
    email: string;
    cpf: string;
    cellphone: string;
    password : string;
    
     -------------------------------------------------------//----------------------------------------------------
     Front-end desenvolvido em Angular.
     
Com a estrutura utilizando Component, Service e Interface.

app.module configurada com imports e declaracao devidamente realziados. 

app-routing.modules com as rotas devidamente criadas inclusive a rota para redirecionamento caso o usuairo digite um link inexixtente.

Enviroment para configracao da URL da API- apiUrl: 'http://localhost:3000'

Interface utilizado para verificar a tipagem dos dados:
    _id: string;
    password: string;
    cellphone: string;
    cpf: string;
    email: string;
    name: string;
    __v: string;
    
Separando as Paginas pela pasta Pages contendo o create, edit e list users.

A pasta Shared para os componentes compartilhados onde contem o componente de erro para notificacao de erro e o form para as telas que compatilham o mesmo form como a tela de create e edit.


Services para realizar a requisicao com a api e enviar os dados para a classe do componente:

 getlistAllUsers():Observable<User[]>{
    const url = `${environment.apiUrl}/users`;
    const headers = new HttpHeaders({
      'Content-Type': 'application/json'
    });
    return this.http.get<User[]>(url, {headers: headers}).pipe(retry(2),catchError(this.handleError));
  }

  getUser(_id:string):Observable<User>{
    const url = `${environment.apiUrl}/users/${_id}`;
    return this.http.get<User>(url);
  }

  addUser(user: User):Observable<User[]>{
    const url = `${environment.apiUrl}/users/`;
    return this.http.post<User[]>(url, user);
  }

  editUser(user: User):Observable<User>{
    const url = `${environment.apiUrl}/users/${user._id}`;
    return this.http.patch<User>(url, user);
  }

  deleteUser(_id:string):Observable<User[]>{
    const url = `${environment.apiUrl}/users/${_id}`;
    return this.http.delete<User[]>(url);
  }

  handleError(error: HttpErrorResponse) {
    let errorMessage = '';
    if(error.error instanceof ErrorEvent){
      errorMessage = error.error.message;
    }else{
      errorMessage = `codigo: ${error.status}, ` + `mensagem: ${error.message}`;
    }
    console.log(errorMessage);
    return throwError(errorMessage);
  }
