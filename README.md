# code-base

```
 public function DoProcessMessages()
        {
            try {
            $this->connectionFactory = PgConnectionFactory::getInstance();
            if ($_SERVER["REQUEST_METHOD"] == "GET") 
            {

                if (isset($_GET["action"]) && $_GET['action']=='getKeyNumber' ) {
                    echo $this->getKeyNumberRecords($_GET["cc"],$_GET["fn"],$_GET["table_name"]);
                    //echo 'response from page';
                    exit();
                }else {
                    $this->processResult = 'Invalid URL';
                }

                if (isset($_GET["action"]) && $_GET['action']=='getFigureNumber' ) {
                    echo $this->getFigureNumberRecords($_GET["cc"],$_GET["maf"],$_GET["table_name"]);
                    //echo 'response from page';
                    exit();
                }else {
                    $this->processResult = 'Invalid URL';
                }

                


                if (isset($_GET["action"]) && $_GET['action']=='getCheckBox' ) {
                    echo $this->getCheckBoxDetails($_GET["cc"],$_GET["fn"]);
                    //echo 'response from page';
                    exit();
                } 
                else {
                    $this->processResult = 'Invalid URL';
                }

                
               
                
            }
           
        }
        catch (Exception $e) {
            $this->processResult = $e->getFile().' -> '.$e->getLine().' -> '.$e->getTraceAsString();
        }
    
        }
    
    
        public function DoExecSQLWithParams($sql, $params) {
            return pg_query_params($this->connection->GetConnectionHandle(), $sql, $params);
        }
    
    
        private function getCheckBoxDetails($cc,$fn) {
            $d = array();
    
            $sql = "SELECT distinct modelcode, catalog_code, concat(modelcode,'-',description ) as display_value FROM bl.bl_na_illust_applmodel where catalog_code = '" . $cc . "' and fig_number = '". $fn . "'";
     
         
    
              $this->Connect();
              //$sql = str_replace('$1', $requestedStepId, $sql);
              $rows = $this->DoExecSQLWithParams($sql, array());
              $i=0;
              $data = array();
              if ($rows && pg_num_rows($rows) > 0) {
                while ($row = pg_fetch_array($rows, null, PGSQL_ASSOC )) {
                    $d = array();
                    $d['modelcode']= $row['modelcode'];
                    $d['catalog_code']= $row['catalog_code'];
                    $d['display_value']= $row['display_value'];
                    $data[$i]=$d;
                    $i=$i+1;
    
                }
                return json_encode($data);
            }
    
        }
    
        private function getKeyNumberRecords($cc,$fn,$table_name) {
            $d = array();
    
            $sql = "SELECT distinct key_number,concat(key_number,'-',part_name) as display_value
            FROM bl.".$table_name." where catalog_code = '" . $cc . "' and fig_number = '". $fn . "'";
     
         
    
              $this->Connect();
              //$sql = str_replace('$1', $requestedStepId, $sql);
              $rows = $this->DoExecSQLWithParams($sql, array());
              $i=0;
              $data = array();
              if ($rows && pg_num_rows($rows) > 0) {
                while ($row = pg_fetch_array($rows, null, PGSQL_ASSOC )) {
                    $d = array();
                    $d['key_number']= $row['key_number'];
                    $d['display_value']= $row['display_value'];
                    $data[$i]=$d;
                    $i=$i+1;
    
                }
                return json_encode($data);
            }
    
        }
       
        private function getFigureNumberRecords($cc,$maf,$table_name) {
            $d = array();
    
            $sql = "SELECT distinct fig_number, concat(fig_number,'-',fig_title) as display_value FROM  bl.".$table_name." where catalog_code = '" . $cc ."' and model_asm_flag = '". $maf . "' ";
     
         
    
              $this->Connect();
              //$sql = str_replace('$1', $requestedStepId, $sql);
              $rows = $this->DoExecSQLWithParams($sql, array());
              $i=0;
              $data = array();
              if ($rows && pg_num_rows($rows) > 0) {
                while ($row = pg_fetch_array($rows, null, PGSQL_ASSOC )) {
                    $d = array();
                    $d['fig_number']= $row['fig_number'];
                    $d['display_value']= $row['display_value'];
                    $data[$i]=$d;
                    $i=$i+1;
    
                }
                return json_encode($data);
            }
    
        }
    
    
    
        function GetConnectionOptions()
        {
            $result = GetGlobalConnectionOptions();
            $result['client_encoding'] = 'utf8';
            GetApplication()->GetUserAuthentication()->applyIdentityToConnectionOptions($result);
            return $result;
        }
    
    
        public function Connect() {
            if (!isset($this->connection)) {
                $this->connection = $this->connectionFactory->CreateConnection($this->GetConnectionOptions());
                $this->connection->Connect();
            }
        }

```
