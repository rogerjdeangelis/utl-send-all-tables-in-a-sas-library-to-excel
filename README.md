# utl-send-all-tables-in-a-sas-library-to-excel
Send all tables in a sas library to excel
    Send all tables in a sas library to excel                                                                                  
                                                                                                                               
    github                                                                                                                     
    https://tinyurl.com/yywjlg9f                                                                                               
    https://communities.sas.com/t5/SAS-Programming/Macro-to-export-each-table-in-a-library-to-an-Excel-file/m-p/542087         
                                                                                                                               
    *_                   _                                                                                                     
    (_)_ __  _ __  _   _| |_                                                                                                   
    | | '_ \| '_ \| | | | __|                                                                                                  
    | | | | | |_) | |_| | |_                                                                                                   
    |_|_| |_| .__/ \__,_|\__|                                                                                                  
            |_|                                                                                                                
    ;                                                                                                                          
                                                                                                                               
    proc datasets lib=work kill;                                                                                               
    run;quit;                                                                                                                  
                                                                                                                               
                                                                                                                               
    data likes;                                                                                                                
        length name $10 likes $10;                                                                                             
        input name $ likes $;                                                                                                  
    cards4;                                                                                                                    
    Peter Cakes                                                                                                                
    Peter Chocolate                                                                                                            
    Simon Basketball                                                                                                           
    Philip Apples                                                                                                              
    Sam Oranges                                                                                                                
    Paul Bananas                                                                                                               
    John Cricket                                                                                                               
    Frank Cats                                                                                                                 
    Frank Evita                                                                                                                
    ;;;;                                                                                                                       
    run;quit;                                                                                                                  
                                                                                                                               
    data dislikes;                                                                                                             
        length name $10 dislikes $10;                                                                                          
        input name $ dislikes $;                                                                                               
    cards4;                                                                                                                    
    Peter Carrots                                                                                                              
    Peter Eggs                                                                                                                 
    Simon Pool                                                                                                                 
    Philip Bananas                                                                                                             
    Sam Pears                                                                                                                  
    Paul Oreos                                                                                                                 
    John Bingo                                                                                                                 
    Frank Rats                                                                                                                 
    Frank Celery                                                                                                               
    ;;;;                                                                                                                       
    ;run;                                                                                                                      
                                                                                                                               
    *            _               _                                                                                             
      ___  _   _| |_ _ __  _   _| |_                                                                                           
     / _ \| | | | __| '_ \| | | | __|                                                                                          
    | (_) | |_| | |_| |_) | |_| | |_                                                                                           
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                          
                    |_|                                                                                                        
    ;                                                                                                                          
                                                                                                                               
    /*                                                                                                                         
    WORK.LOG total obs=2                                                                                                       
                                                                                                                               
      NAME        RC               STATUS                                                                                      
                                                                                                                               
      DISLIKES     0    created d:/xls/DISLIKES.xlsx                                                                           
      LIKES        0    created d:/xls/LIKES.xlsx                                                                              
                                                                                                                               
                                                                                                                               
                                                                                                                               
    NOTE: Libref XEL was successfully assigned as follows:                                                                     
          Engine:        EXCEL                                                                                                 
          Physical Name: d:/xls/DISLIKES.xlsx                                                                                  
    SYMBOLGEN:  Macro variable NAME resolves to DISLIKES                                                                       
    NOTE: The data set XEL.DISLIKES has 9 observations and 2 variables.                                                        
                                                                                                                               
                                                                                                                               
    NOTE: Libref XEL was successfully assigned as follows:                                                                     
          Engine:        EXCEL                                                                                                 
          Physical Name: d:/xls/LIKES.xlsx                                                                                     
    SYMBOLGEN:  Macro variable NAME resolves to DISLIKES                                                                       
    NOTE: The data set XEL.LIKES has 9 observations and 2 variables.                                                           
    */                                                                                                                         
                                                                                                                               
    *          _       _   _                                                                                                   
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                        
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                       
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                      
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                      
                                                                                                                               
    ;                                                                                                                          
                                                                                                                               
    %symdel name / nowarn; * in case it is already global;                                                                     
                                                                                                                               
    data log ;                                                                                                                 
                                                                                                                               
      if _n_=0 then do; %let rc=%sysfunc(dosubl('                                                                              
         ods exclude _all_;                                                                                                    
         ods output members=dslist(keep=name);                                                                                 
         proc contents data=work._all_  mt=data;                                                                               
         run;quit;                                                                                                             
         ods select _all_;                                                                                                     
         run;quit;                                                                                                             
         '));                                                                                                                  
      end;                                                                                                                     
                                                                                                                               
      set dslist;                                                                                                              
                                                                                                                               
      call symputx("name",name);                                                                                               
                                                                                                                               
      rc=dosubl('                                                                                                              
         %utlfkil(d:/xls/&name..xlsx);                                                                                         
         libname xel "d:/xls/&name..xlsx";                                                                                     
         data xel.&name;                                                                                                       
            set &name;                                                                                                         
         run;quit;                                                                                                             
         %let cc=&syserr;                                                                                                      
      ');                                                                                                                      
                                                                                                                               
      if symgetn('cc')=0 then status=cats(" ","created d:/xls/",name,".xlsx");                                                 
      else status=cats(" ","failed d:/xls/",name,".xlsx");                                                                     
                                                                                                                               
    run;quit;                                                                                                                  
                                                                                                                               
                                                                                                                               
