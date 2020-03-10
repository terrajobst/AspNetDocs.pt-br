> Alguns comandos não têm suporte se o aplicativo usa o SQLite como seu armazenamento de dados de identidade. Devido a limitações no mecanismo de banco de dados, `Alter` comandos lançam a seguinte exceção:
>
> "System. NotSupportedException: o SQLite não dá suporte a esta operação de migração." 
>
> Como solução alternativa, execute Code First migrações no banco de dados para alterar as tabelas.
