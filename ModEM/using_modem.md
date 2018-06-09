# Modular System Eletromanetic - ModEM
**Rotina para modelagem e inversão .**


## 3D grid
Interface gráfica utilzada para geração do modelo, dado e covariância no formato do ModEM, servirá também para visualização dos resultados.

* *Passos iniciais:
Importar com os dados (edi), criar modelo, centralizar as estações (certificar que cada estação está ocupando apenas uma célula), incluir topografia e/ou batimetria, incluir oceano (para investigações próximas a costa).

Após a modelagem, deve-se:
>* Salvar o modelo (large model).
>* Salvar o modelo reduzido (nested model) - para casos em que queira otimizar a inversão.
>+ Salvar o arquivo de covariância, se houver modificações de resistividade ou suavização.
>+ Salvar os dados no formato ModEM
>+ Salvar os dados sintéticos (obter layout dos períodos).

## ModEM

### Forward Modelling
* basic

`../ModEM -F input_model input_data output_Resp_large_model`

* Advanced

`../ModEM -F input_model input_data output_Resp_large_model` **`output_E_solution FWD_parameters`**

Output_E_solution (Binary format)
Is a file in which we store the electric field components (Ex, Ey, Ez) at all cell edges for all periods and both polarization,
is necessary to perform later e.g., the nested modeling.

FWD_para (ASCII format)
An Ascii file in which we define parameters that control the solver 
It contains the name of Output_E_solution which will be used for the nested modeling.

* Run the forward modelling for the nested model:

`../ModEM –F nested_model data_file Resp_large_model e_solution_tmp FWD_para`

*At the some point after executing ModEM to run the forward modelling or the inversion with nested modelling enabled (e_solution file name is inside FWD_para file)*

### Inversion

* Basic Input/Output
`../ModEM –I NLCG Input_Model Input_Data`
 
* Advanced-level1
`../ModEM -I NLCG Input_Model Input_data Input_INV_para.`
 
* Advanced-level2
`../ModEM –I  NLCG Input_Model Input_data Input_INV_para. FWD_para.`
 
* Advanced-level3
`Input_Model Input_data Input_INV_para. FWD_para. Model_Cov`
 
* Advanced-level4
`../ModEM –I  NLCG Input_Model Input_data Input_INV_para. FWD_para. Model_Cov model_prm`
 

- **Input_Model Input_data Input_INV_para.**
* Example: Input_INV_para (ASCII Format)
Information in this file controls the inversion process
Each line has an explanation of what are these information for:

>- Model and data output file name : ModEM3DMT;
>- Initial daming factor lambda: 1.0;
>- Restart when rms diff is less than: 2.0e-3;
>- Exit search when rms is less than: 1.05;
>- Exit when lambda is lesse than: 1.0e-8;
>- Maximun number of iterations : 100;

- **Input_Model Input_data Input_INV_para. FWD_para. Model_Cov model_prm**
To use the optional model perturbation file (model_prm) as an input we need first to understand few things:
The output files after each iteration are:  
***_NLCG_060.rho  > Inverted model   
***_NLCG_060.dat  > Predicted data 
***_NLCG_060.prm  > Transformed model parameter (rough)
***_NLCG_060.res  > Data residuals



**Para uso no clustercoge** o seguinte comando está sendo usado:

**`> srun -N [1]  -o out.txt -e out.error -n [2] --mpi=pmi2 Mod3DMT -I NLCG arq.mod arq.dat ctrl.inv ctrl.fwd arq.cov arq.prm &`**


* [1] é referente ao valor 10, 15 ou 97 *(ainda não sei a diferença o 15 funciona)*
* [2] é referente ao valore resultante do = (N.Periodos x 2 ) + 1

#