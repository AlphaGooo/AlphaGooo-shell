
执行脚本，根据提示和选择输入得到相应的响应和执行结果。

```
#!/bin/bash
##############################
# @desc 重新清洗ERP数据
# @file yyerp_update_sync_data.sh
# @author AlphaGooo
# @version 1.0
# @date 2017-04-20
##############################

# 确认是否执行，避免误操作
read -r -p "Are You Sure? [Y/n] " input

case $input in
    [yY][eE][sS]|[yY])
        echo "yes! just do it..."

        # 操作数据库 清理数据
        mysql -u root -pjqcbq1w2e3 jekun_manager -e "
        update crontab_data set late_updated_at = '' where type = 'yyerp_sync_user_type_by_time';
        truncate table yyerp_user_type;
        "

        # 执行脚本 同步数据
        sudo /home/http/jekunauto.com/src/yii yyerp/user-type/sync-timed
        ;;

    [nN][oO]|[nN])
        echo "No, exit..."
            ;;

    *)
    echo "Invalid input..."
    exit 1
    ;;
esac
```
