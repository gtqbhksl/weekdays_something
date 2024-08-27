https://github.com/master-nan/sweet-cms

## 1. Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection') in /table/index

In line 63 of the repository/impl/ sys_table_imp.go file, you use fmt.Sprintf to build SQL statements directly, including user-supplied values (via the parameters indexName, tableName, and fields). If an attacker is able to control any of these parameters, they could inject malicious SQL code into them to manipulate database queries or perform other malicious operations.

code flow:

from route /table/index

```
代码 initialize/router.go 片段(行 74 到 74 )：
		adminGroup.POST("/table/index", app.TableController.CreateTableIndex)

代码 controller/sys_table_controller.go 片段(行 339 到 339 )：
func (t *TableController) CreateTableIndex(ctx *gin.Context) {

代码 controller/sys_table_controller.go 片段(行 344 到 344 )：
	err := utils.ValidatorBody[request.TableIndexCreateReq](ctx, &data, translator)

代码 controller/sys_table_controller.go 片段(行 349 到 349 )：
	err = t.sysTableService.CreateTableIndex(ctx, data)

代码 service/sys_table_service.go 片段(行 555 到 555 )：
func (s *SysTableService) CreateTableIndex(ctx *gin.Context, req request.TableIndexCreateReq) error {

代码 service/sys_table_service.go 片段(行 556 到 556 )：
	err := s.sysTableRepo.ExecuteTx(ctx, func(tx *gorm.DB) error {

代码 service/sys_table_service.go 片段(行 592 到 592 )：
		if e := s.sysTableRepo.CreateTableIndex(tx, req.IsUnique, req.IndexName, table.TableCode, fields); e != nil {

代码 repository/impl/sys_table_impl.go 片段(行 56 到 56 )：
func (s *SysTableRepositoryImpl) CreateTableIndex(tx *gorm.DB, isUnique bool, indexName string, tableCode string, fields string) error {

代码 repository/impl/sys_table_impl.go 片段(行 62 到 62 )：
	createIndexSql := fmt.Sprintf("CREATE %s INDEX %s ON %s (%s)", unique, indexName, tableName, fields)

代码 repository/impl/sys_table_impl.go 片段(行 62 到 62 )：
	createIndexSql := fmt.Sprintf("CREATE %s INDEX %s ON %s (%s)", unique, indexName, tableName, fields)

代码 repository/impl/sys_table_impl.go 片段(行 63 到 63 )：
	return tx.Exec(createIndexSql).Error
```

```
type TableIndexCreateReq struct {
	TableId     int                  `json:"table_id" binding:"required"`
	IndexName   string               `json:"index_name" binding:"required"`
	IsUnique    bool                 `json:"is_unique" binding:"required"`
	IndexFields []TableIndexFieldReq `json:"index_fields" binding:"required,min=1"`
}
```


## 2. Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection') in /table/index/:id

Vulnerability Description: The query relies on user-supplied values. In this case, the problem is with the dynamically constructed dropIndexSQL statement. Using user-supplied 'indexName' and 'tableName' to insert directly into SQL statements without proper validation and/or escape can lead an attacker to insert malicious SQL code that can perform illegal database operations.


code flow:

from route /table/index/:id

```
代码 initialize/router.go 片段(行 75 到 75 )：
		adminGroup.PUT("/table/index/:id", app.TableController.UpdateTableIndex)

代码 controller/sys_table_controller.go 片段(行 357 到 357 )：
func (t *TableController) UpdateTableIndex(ctx *gin.Context) {

代码 controller/sys_table_controller.go 片段(行 362 到 362 )：
	err := utils.ValidatorBody[request.TableIndexUpdateReq](ctx, &data, translator)

代码 controller/sys_table_controller.go 片段(行 367 到 367 )：
	err = t.sysTableService.UpdateTableIndex(ctx, data)

代码 service/sys_table_service.go 片段(行 602 到 602 )：
func (s *SysTableService) UpdateTableIndex(ctx *gin.Context, req request.TableIndexUpdateReq) error {

代码 service/sys_table_service.go 片段(行 603 到 603 )：
	err := s.sysTableRepo.ExecuteTx(ctx, func(tx *gorm.DB) error {

代码 service/sys_table_service.go 片段(行 616 到 616 )：
		if e := s.sysTableRepo.DropTableIndex(tx, req.IndexName, table.TableCode); e != nil {

代码 repository/impl/sys_table_impl.go 片段(行 67 到 67 )：
func (s *SysTableRepositoryImpl) DropTableIndex(tx *gorm.DB, indexName string, tableCode string) error {

代码 repository/impl/sys_table_impl.go 片段(行 70 到 70 )：
	dropIndexSQL := fmt.Sprintf("DROP INDEX %s ON %s", indexName, tableName)

代码 repository/impl/sys_table_impl.go 片段(行 70 到 70 )：
	dropIndexSQL := fmt.Sprintf("DROP INDEX %s ON %s", indexName, tableName)

代码 repository/impl/sys_table_impl.go 片段(行 71 到 71 )：
	return tx.Exec(dropIndexSQL).Error

```

## 3.Improper Output Neutralization for Logs in	middleware/log.go

Cause of vulnerability: In lines 58-62 of the file 'middleware/log.go', the log entry relies on a user-supplied value (' c.reequest '). If the values provided by the user contain special characters or malicious code, these values may be logged directly into the log, resulting in log injection attacks. Attackers may manipulate the content of logs by submitting specific requests, which may lead to security issues such as leakage of sensitive information and misuse of log files. This is a typical log injection vulnerability.

middleware/log.go line 57-65

```
		zap.L().Info("用户访问日志:",
			zap.String("uri", c.Request.URL.Path),
			zap.String("method", c.Request.Method),
			zap.Any("query", c.Request.URL.Query()),
			zap.Any("body", c.Request.Body),
			zap.Any("response", responseBody),
			zap.String("ip", c.ClientIP()),
			zap.String("duration", fmt.Sprintf("%.4f seconds", duration.Seconds())))
		zap.L().Info("Access Log end")
```
