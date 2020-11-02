# utl-using-sql-arrays-to-implement-datastep-variable-ranges
Using sql arrays to implement datastep variable ranges 
    Using sql arrays to implement datastep variable ranges                                                                    
                                                                                                                              
    I have the following variables                                                                                            
                                                                                                                              
       JAN FEB MAR APR MAY JUN JUL AUG SEP OCT NOV DEC                                                                        
                                                                                                                              
     and would like to select variables in the range JUL--DEC.                                                                
                                                                                                                              
    USING JUST A SQL SELECT CLAUSE.                                                                                           
                                                                                                                              
                                                                                                                              
    GitHub                                                                                                                    
    https://cutt.ly/sgPHC1C                                                                                                   
    https://github.com/rogerjdeangelis/utl-using-sql-arrays-to-implement-datastep-variable-ranges                             
                                                                                                                              
    SAS Forum                                                                                                                 
    https://cutt.ly/NgPHBai                                                                                                   
    https://communities.sas.com/t5/SAS-Programming/Proc-Sql-to-import/m-p/694867                                              
                                                                                                                              
    macros                                                                                                                    
    https://tinyurl.com/y9nfugth                                                                                              
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                                
                                                                                                                              
                                                                                                                              
    *                                                                                                                         
    #####  #   #  ####   #   #  #####                                                                                         
      #    ##  #  #   #  #   #    #                                                                                           
      #    # # #  #   #  #   #    #                                                                                           
      #    #  ##  ####   #   #    #                                                                                           
      #    #   #  #      #   #    #                                                                                           
      #    #   #  #      #   #    #                                                                                           
    #####  #   #  #       ###     #                                                                                           
                                                                                                                              
    #! INPUT ;                                                                                                                
                                                                                                                              
    data have;                                                                                                                
                                                                                                                              
       array mths[12] JAN FEB MAR APR MAY JUN JUL AUG SEP OCT NOV DEC;                                                        
                                                                                                                              
       do year=1950 to 1955;                                                                                                  
          do idx=1 to dim(mths);                                                                                              
              mths[idx]=int(900*uniform(7654))+100;                                                                           
          end;                                                                                                                
          output;                                                                                                             
       end;                                                                                                                   
                                                                                                                              
       drop idx;                                                                                                              
                                                                                                                              
    run;quit;                                                                                                                 
                                                                                                                              
    WORK.HAVE total obs=6                                                                                                     
                                                                                                                              
      JAN    FEB    MAR    APR    MAY    JUN    JUL    AUG    SEP    OCT    NOV    DEC    YEAR                                
                                                                                                                              
      733    153    329    493    951    863    966    611    211    550    157    734    1950                                
      892    307    154    750    613    749    242    719    786    365    255    482    1951                                
      722    577    682    686    171    694    891    589    557    536    334    949    1952                                
      134    975    181    593    631    493    941    773    283    521    300    813    1953                                
      664    772    922    266    533    366    988    441    823    351    390    869    1954                                
      381    206    856    499    363    667    871    573    504    790    284    200    1955                                
                                                                                                                              
    *                                                                                                                         
     ###   #   #  #####  ####   #   #  #####                                                                                  
    #   #  #   #    #    #   #  #   #    #                                                                                    
    #   #  #   #    #    #   #  #   #    #                                                                                    
    #   #  #   #    #    ####   #   #    #                                                                                    
    #   #  #   #    #    #      #   #    #                                                                                    
    #   #  #   #    #    #      #   #    #                                                                                    
     ###    ###     #    #       ###     #                                                                                    
                                                                                                                              
    #! OUTPUT ;                                                                                                               
                                                                                                                              
    WORK.WANT total obs=6                                                                                                     
                                                                                                                              
      JUL    AUG    SEP    OCT    NOV    DEC                                                                                  
                                                                                                                              
      966    611    211    550    157    734                                                                                  
      242    719    786    365    255    482                                                                                  
      891    589    557    536    334    949                                                                                  
      941    773    283    521    300    813                                                                                  
      988    441    823    351    390    869                                                                                  
      871    573    504    790    284    200                                                                                  
                                                                                                                              
    *                                                                                                                         
    ####   ####    ###    ###   #####   ###    ###                                                                            
    #   #  #   #  #   #  #   #  #      #   #  #   #                                                                           
    #   #  #   #  #   #  #      #       #      #                                                                              
    ####   ####   #   #  #      ####     #      #                                                                             
    #      # #    #   #  #      #         #      #                                                                            
    #      #  #   #   #  #   #  #      #   #  #   #                                                                           
    #      #   #   ###    ###   #####   ###    ###                                                                            
                                                                                                                              
    #! PROCESS ;                                                                                                              
                                                                                                                              
    %array(vrs,values=%varlist(have,keep=JUL--DEC));                                                                          
                                                                                                                              
    proc sql;                                                                                                                 
      create                                                                                                                  
         table want as                                                                                                        
      select                                                                                                                  
         %do_over(vrs,phrase=?,between=comma)                                                                                 
      from                                                                                                                    
         have                                                                                                                 
    ;quit;                                                                                                                    
                                                                                                                              
                                                                                                                              
                                                                                                                              
