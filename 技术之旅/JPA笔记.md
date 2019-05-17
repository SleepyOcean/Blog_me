# JPA笔记

* sql.Date保存时间只精确到年月日，使用sql.Timestamp可以精确到时分秒

* JPA中的save和saveAll方法是面向业务的，每次插入都需要验证当前数据是否为新数据，所以如果是批量插入请勿使用save方法，会做许多无用判断。

* @Query注解处理的是JPA实体和实体中的属性，不同于SQL直接处理表、字段。

* 查询时不需要@Modifying注解。@Modifying:指示方法应被视为修改查询。

  ``` java
  	//更新操作
  	@Modifying
      @Query("update VehicleDeckEntity hvd set hvd.deckStatus = ?1 where hvd.vehicleUniqueKey = ?2")
      void updateDeckStatusByVehicleUniqueKeyWithSQL(Integer deckStatus, String vehicleUniqueKey);

  	//查询操作
      @Query(value = "SELECT * FROM hyAccidentFileMan WHERE HPHM =?1", nativeQuery = true)
      List<AccidentFileManEntity> findAllByHphmWithSQL(String hphm);
  ```

  ​


