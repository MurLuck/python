# python
<form method="get" action="index.php">
    location:<input name="location" type="Text"><br>
    max steps(min:10 max:20):<input name="step_limit" type="number" min="10" max="20" value="10"><br>
    <input name="display_pokestops" type="checkBox" value="true"> Display Pokestops<br>
    <input name="only_lure_pokestops" type="checkBox" value="true"> Display Only Lured Pokestops<br>
    <input name="display_gym" type="checkBox" value="true"> Display Gyms<br>
    <br>
    <input type="submit" name="search" value="Submit"><br>
    <br>
    </form> 
    

<script>
    function showMessage(value){
        window.location = "http://pokemap.noip.me"
        alert(value + "is missing")
    }
</script>

<?php

        if (!empty($_GET['search']))
        {
            if (empty($_GET['location'])){
                echo '<script type="text/javascript">showMessage("location");</script>';
                exit;
            }
            
            $location = $_GET['location'];

            if (empty($_GET['step_limit']))
            {
                echo '<script type="text/javascript">showMessage("steps");</script>';
                exit;
            }

            $max_steps = $_GET['step_limit'];

            $backGroundCommand = ' > /dev/null 2>/dev/null &';
            $command = 'python example.py -H 10.0.0.10 -P 8001 -a ptc -u murluck -p 13081989 -l "'.$location.'" -st '.$max_steps.' -ar 10';
            $kill_Command = "pkill -f example.py";

            if (!empty($_GET['display_pokestops']))
            {
                $command = $command.' -dp ';
            }

            if (!empty($_GET['display_gym']))
            {
                $command = $command.' -dg ';
            }

            if (!empty($_GET['only_lure_pokestops']))
            {
                $command = $command.' -ol ';
            }

            $command = $command.$backGroundCommand;

            $old_dir = getcwd();
            $new_dir = '/Applications/XAMPP/xamppfiles/htdocs/PokemonGo-Map-master';
            chdir($new_dir);

            $killprint = shell_exec($kill_Command);
            shell_exec($command);
            sleep(2);
            
            ?>

                <iframe src="http://pokemap.noip.me:8001/" width="99%" height="80%" allowfullscreen></iframe>
            <?php
            chdir($old_dir);
        }
?>
