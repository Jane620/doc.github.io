Mongodb基础
===========================

介绍
~~~~~~~~~~~~~~~~~~~
mongodb是一个NoSQL




Java调用
~~~~~~~~~~~~~~~~~~~~~~


多字段查询
::

    Criteria criteria = new Criteria();
    criteria.orOperator(
                        Criteria.where("feeCode").is(feeCode),
                        Criteria.where("refFeeAct").is(feeCode),
                        Criteria.where("refFeeRed").is(feeCode),
                        Criteria.where("refFeeRev").is(feeCode)
    );
